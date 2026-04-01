---
name: buddy-rename
description: Rename Claude Code buddy (companion). Use when user requests "rename buddy", "change buddy name", etc.
---

# Buddy Rename

Changes the name of the Claude Code companion (buddy).

## Usage

If the user passes a new name as an argument, it is applied immediately.
If no argument is provided, the user is prompted for the desired name.

## Execution

```bash
python3 -c "
import json, sys

name = sys.argv[1] if len(sys.argv) > 1 else input('New buddy name: ')
name = name.strip()
if not name:
    print('Name is empty.')
    sys.exit(1)
if len(name) > 14:
    print(f'Name is too long (max 14 chars, current {len(name)} chars)')
    sys.exit(1)

with open('$HOME/.claude.json') as f:
    d = json.load(f)

old_name = d.get('companion', {}).get('name', '(none)')
d.setdefault('companion', {})['name'] = name

with open('$HOME/.claude.json', 'w') as f:
    json.dump(d, f, indent=2, ensure_ascii=False)

print(f'Buddy renamed: {old_name} → {name}')
print('Restart Claude Code to apply the change.')
" "$ARGS"
```

## Notes

- Buddy name is limited to 14 characters max
- Species (shape) is fixed based on account UUID and cannot be changed
- To change personality, manually edit `companion.personality` in `~/.claude.json`
