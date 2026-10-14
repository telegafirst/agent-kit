# Plan from the playbook catalog

Use this before building any funnel, event, booking or knowledge setup. The server carries the know-how, so it works the same with or without this package.

1. Call `list_playbooks`. Pick the scenario closest to the owner's request: webinar or event, consultation, tripwire, marathon. If none fits, take the nearest one and say which part you change.
2. Call `get_playbook` with its id. Read the assembly order, branches, how the funnel ends, the common mistakes and the self-check. The example chain in the answer already passes the chain-step schema: copy its shape, replace the texts and numbers.
3. For every block in the order call `get_block` with its id. It lists the tools in call order with required fields, what the block needs from earlier blocks and what it leaves for later ones, and its settings.
4. Show the owner the plan: which blocks, the full text of every message, dates, prices and files you still need. Change nothing before the owner agrees.
5. Build block by block in the stated order. After each block read the result back with the read tool of that block.
6. Finish with the playbook self-check and report what was created by its tenant-visible numbers.

The same catalog is published as the public page `mcp-playbooks`; the tools answer in the business language.
