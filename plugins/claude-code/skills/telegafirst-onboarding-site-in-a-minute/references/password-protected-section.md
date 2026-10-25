# A section behind a password (an offer, reports, a course)

Use it when part of the site must open only to people who know a password: a commercial offer for one client, monthly reports, a course for a group. The password lives on the platform. Never put a password in HTML or JS: a page that compares what the visitor types with a text written in it protects nothing, because anyone reads the page source, and `site_publish` reports such a page.

## The sequence

1. Build the closed section as ordinary pages of the bundle under one folder, for example `offer/`, and publish it with `site_publish` like any page. Publishing never changes access rules.
2. The owner chooses the password. Call `site_password_set` with a lowercase **name** (letters, digits, hyphens; it is how rules refer to the password), the **password** (8 to 128 characters) and, if wanted, **remember_days** (1 to 365) and a **note** of who it is for (up to 200 characters). A visitor who signs in is remembered for 30 days unless you set another number.

```json
{ "name": "acme", "password": "PASSWORD_FROM_THE_OWNER", "remember_days": 30, "note": "Acme LLC, Ivan" }
```

3. Close the folder with `site_gate_set`: a path mask ending in `/*`, and **any_of** with the password by name. Without a **block** the whole page is closed and a visitor without access is sent to the sign-in page.

```json
{ "path": "/offer/*", "any_of": [{ "kind": "password", "password": "acme" }] }
```

4. Check with `site_gate_tree` for the host (`site_list_pages` names the hosts): every page of the folder lists the rule. Open a closed page in a private window: the sign-in page appears; after the password the page opens.
5. Tell the owner to hand the password to the audience. The sign-in is checked against only a hash, and no listing or log shows the password: `site_password_list` gives the name, the note and the sections it opens. To remind a client of a password or put it into a letter to the customer, call `site_password_reveal` with its name; every reveal is recorded in the audit log. To change it, call `site_password_set` with the same name: everyone who entered with the old one is signed out. A password a rule names is removed only after those rules are changed or removed.

The sign-in page is ready: it shows the business name and colors, signs in without Enter and works in light and dark theme. Do nothing for it. Build your own only when the owner asks for a designed page (below).

## What the rule does

- **any_of** lists up to 10 conditions and one of the conditions is enough: a password, a bought product, a subscription to the business channel, a segment.
- When several masks match a path, the most specific mask decides: `/kp/acme/*` wins over `/kp/*`. The general rule does not also apply.
- A sign-in is remembered per password, not per folder. Two folders that accept the same password are one sign-in. Signing in with another password opens the folders that accept that one.

## Several clients, one folder each

A shared password opens everything under `/kp/`; each client also has a password for its own folder only.

```json
{ "path": "/kp/*", "any_of": [{ "kind": "password", "password": "shared" }] }
```

```json
{ "path": "/kp/acme/*", "any_of": [{ "kind": "password", "password": "acme" }, { "kind": "password", "password": "shared" }] }
```

`/kp/acme/offer.html` opens with either password; `/kp/beta/offer.html` opens only with the shared one. Make both passwords with `site_password_set` first. The rule of `/kp/acme/*` replaces the general one for that folder, so it lists the shared password too.

## An own sign-in page

Only when a designed page is wanted. Put the page and its script in the bundle as files, outside every closed mask (`/login.html`, not `/offer/login.html`), and name it in the rule with **login_path**.

```json
{ "path": "/offer/*", "any_of": [{ "kind": "password", "password": "acme" }], "login_path": "/login.html" }
```

`login.html`:

```html
<form id="login" autocomplete="off">
  <label for="pw">Password</label>
  <input id="pw" type="password" autocomplete="current-password" autofocus>
  <p id="error" role="alert" hidden>Wrong password</p>
</form>
<script src="/widget/gate.v1.js" defer></script>
<script src="/js/login.js" defer></script>
```

`js/login.js`, your own file of the bundle:

```js
var input = document.getElementById('pw');
var error = document.getElementById('error');
var timer;

function submit() {
  tgf.gate.unlock(input.value).then(function (result) {
    if (result.ok) {
      location.replace(result.next);
      return;
    }
    error.textContent = result.retryAfter
      ? 'Too many attempts. Try again in ' + result.retryAfter + ' s.'
      : 'Wrong password';
    error.hidden = false;
  });
}

input.addEventListener('input', function () {
  error.hidden = true;
  clearTimeout(timer);
  if (input.value.length >= 8) timer = setTimeout(submit, 400);
});

document.getElementById('login').addEventListener('submit', function (event) {
  event.preventDefault();
  submit();
});
```

`tgf.gate.unlock(password)` answers with **ok**, and **next**, the page the visitor came for, taken from the address of the sign-in page. It redirects nothing: the page does `location.replace(result.next)` itself, so an animation fits in between.

A closed page gets a Sign out button the same way: `<button id="logout" type="button">Sign out</button>`, the platform script and a file of the bundle.

```js
document.getElementById('logout').addEventListener('click', function () {
  tgf.gate.logout().then(function () { location.assign('/'); });
});
```

`tgf.gate.logout()` signs out of the passwords of the section the page belongs to; other folders with other passwords stay open. `tgf.gate.logout({ all: true })` signs out of every password on the host.

A **block** can be closed by a password too: `site_gate_set` with the page mask, the block name and **any_of** with the password. The visitor sees the block card, its button opens the sign-in page with `?block=<name>`, and after the right password the block opens on that page. `tgf.gate.logout({ block: "<name>" })` signs out of that block only.

## Data for the admitted, and keys of other services

There are no environment variables of a page. Data only the admitted may read is a JSON file of the bundle under the closed mask: a rule on `/kp/acme/*` closes `/kp/acme/data.json` too, and a visitor without access never receives the file. A page that signed in reads it with a plain `fetch('/kp/acme/data.json')`; the refusal for a visitor without access is a 401, not a page.

A key of another service goes in with `secret_put` and is used by a form handler or a relay on the platform, never by a script in the page. A formula that runs in the browser cannot be hidden from everyone: whatever the browser runs, the visitor can read, so keep it out of the page or keep the page behind the password.
