# _template/

Placeholder content for every gitignored folder. Used so this OS can be shared as a public/template repo without leaking your private content.

## How it works

When a new user runs `/setup` for the first time, the skill copies from `_template/` into the live folders:

```
_template/company/about.md         →  company/about.md
_template/products/example/        →  (used as reference, not copied)
_template/identity/voice-profile.md →  identity/voice-profile.md
```

After the copy, the user fills in the live versions through the `/setup` modules. The `_template/` originals stay unchanged.

## Don't edit `_template/` for personal use

If you want to change the OS's behavior or improve a template, edit:
- The live folders (your actual content)
- `templates/` (artifact scaffolds)
- `.claude/skills/<skill>/SKILL.md` (skill behavior)

`_template/` is only for the *shareable, generic* version — what someone else's first-run state should look like. Edit it only if you're improving the OS itself for distribution.

## What's in here

Mirror of the gitignored structure, with placeholder content:

```
_template/
├── company/                    # Generic placeholders for company files
├── products/example/           # An example product to show the structure
├── identity/                   # Generic identity placeholder
│   ├── about-me.md
│   ├── voice-profile.md
│   ├── values.md
│   └── examples/               # Empty stubs
└── knowledge.md                # Empty knowledge file
```
