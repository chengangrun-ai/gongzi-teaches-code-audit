---
name: gongzi-teaches-code-audit
description: >
  【全仓过度设计审计】ponytail-review 的整仓版：扫整个代码库而不是单次改动，
  按「能砍掉多少」从大到小排名输出清单。只出报告，不改代码。
  触发词：/ponytail-audit、审计整个项目、这个仓库能删什么、找膨胀。
---

ponytail-review, repo-wide. Scan the whole tree instead of a diff. Rank
findings biggest cut first.

## Tags

Same as ponytail-review:

- `delete:` dead code, unused flexibility, speculative feature. Replacement: nothing.
- `stdlib:` hand-rolled thing the standard library ships. Name the function.
- `native:` dependency or code doing what the platform already does. Name the feature.
- `yagni:` abstraction with one implementation, config nobody sets, layer with one caller.
- `shrink:` same logic, fewer lines. Show the shorter form.

## Hunt

Deps the stdlib or platform already ships, single-implementation interfaces,
factories with one product, wrappers that only delegate, files exporting one
thing, dead flags and config, hand-rolled stdlib.

## Output

One line per finding, ranked: `<tag> <what to cut>. <replacement>. [path]`.
End with `net: -<N> lines, -<M> deps possible.` Nothing to cut: `Lean already. Ship.`

## Boundaries

Scope: over-engineering and complexity only. Correctness bugs, security holes,
and performance are explicitly out of scope. Route them to a normal review
pass. Lists findings, applies nothing. One-shot.
"stop ponytail-audit" or "normal mode" to revert.

---

## 来源

本项目基于 [Ponytail](https://github.com/DietrichGebert/ponytail)（MIT License，作者 DietrichGebert）改写，
保留了原作者的版权声明（见 `LICENSE`）。核心思路一致，表述与结构按中文语境重写。

触发词：整仓审计、这个仓库能删什么、找膨胀
