English | [中文](README.md)

# gongzi-teaches-code-audit

An agent skill that scales the diff review up to the whole repo: it scans every file and ranks what can be deleted **by how much it saves**, biggest cut first.

Report only. It does not change your code.

![整仓审计：按能砍多少排名](screenshots/hero.png)

## Why it exists

A diff review only sees the few dozen lines you just touched. Real bloat accumulates — an abstraction from three months ago, a config nobody sets, a factory with one product, scattered across dozens of files that nobody has time to read.

This skill is the one with time. Findings are ranked by payoff, so the first line tells you whether it's worth starting.

## What it does

1. Scans the whole tree
2. Tags each finding with the same five tags as the diff review: `delete:` / `stdlib:` / `native:` / `yagni:` / `shrink:`
3. One line per finding, ranked biggest first: `<tag> <what to cut>. <replacement>. [path]`
4. Ends with `net: -N lines, -M deps possible.`

Nothing to cut? It says `Lean already. Ship.`

## What it hunts for

- Dependencies the standard library or platform already ships
- Interfaces with a single implementation
- Factories with one product
- Wrappers that only delegate
- Files exporting one thing
- Dead flags and config nobody sets
- Hand-rolled standard library

## What it looks like

```
delete: src/legacy/sync.py, all 412 lines, no references. Nothing replaces it.  [src/legacy/]
native: lodash used for deep clone. structuredClone().  [src/utils/clone.js]
yagni: IRepository with a single SqlRepo implementation. Inline until a second store exists.  [src/db/]
shrink: hand-rolled pagination slice. arr.slice(i, i+n).  [api/list.py]

net: -520 lines, -2 deps possible.
```

## Install

```bash
git clone https://github.com/chengangrun-ai/gongzi-teaches-code-audit.git \
  ~/.workbuddy-ai/skills/gongzi-teaches-code-audit
```

No dependencies, no pip install.

## Usage

```
/ponytail-audit
```

Or just ask: "audit the whole project" / "what can be deleted from this repo?"

## Boundaries

- Over-engineering and complexity only. Correctness bugs, security, and performance are out of scope.
- Lists findings, applies nothing. If you want it to actually cut code, say so explicitly.
- One-shot. To exit: say "normal mode" or `stop ponytail-audit`.

## Credits

Rewritten from [Ponytail](https://github.com/DietrichGebert/ponytail) (MIT, by DietrichGebert). The original copyright notice is preserved in `LICENSE`.

## License

MIT. See [LICENSE](LICENSE).

Made by [Gongzi](https://github.com/chengangrun-ai).
