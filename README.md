中文 | [English](README.en.md)

# 公子教代码做事 · 整仓审计

一个 agent 技能，把「改动审查」放大到整个仓库：扫一遍全部代码，把能删的东西按**能砍掉多少**从大到小排名，列成一张清单。

只出报告，不改代码。

## 为什么有它

改动审查只能看见你这次动的那几十行。真正的膨胀是攒出来的——三个月前的抽象、没人设过的配置、只有一个实现的工厂，散落在几十个文件里，谁也没空逐个看。

这个技能就是那个「有空逐个看」的人。它按收益排序，所以你看第一行就知道值不值得动手。

## 它做什么

1. 扫整棵代码树
2. 每条发现打标签（和改动审查同一套）：`delete:` / `stdlib:` / `native:` / `yagni:` / `shrink:`
3. 一行一条，按「能砍多少」从大到小排：`<标签> <砍什么>. <用什么替代>. [路径]`
4. 结尾一行 `net: -N lines, -M deps possible.`

没得砍就说 `Lean already. Ship.`

## 它重点找什么

- 标准库或平台已经提供的依赖
- 只有一个实现的接口
- 只有一个产品的工厂
- 只做转发的包装层
- 只导出一个东西的文件
- 没人用的开关和配置
- 手写的标准库

## 长什么样

```
delete: src/legacy/sync.py, 全部 412 行，无人引用。 无替代。  [src/legacy/]
native: 用 lodash 做 deep clone。 structuredClone()。  [src/utils/clone.js]
yagni: IRepository + 唯一实现 SqlRepo。 内联，等第二个存储出现。  [src/db/]
shrink: 手写分页切片。 arr.slice(i, i+n)。  [api/list.py]

net: -520 lines, -2 deps possible.
```

## 安装

```bash
git clone https://github.com/chengangrun-ai/gongzi-teaches-code-audit.git \
  ~/.workbuddy-ai/skills/gongzi-teaches-code-audit
```

没有依赖，不用 pip install。

## 用法

```
/ponytail-audit
```

或者直接说「审计整个项目」「这个仓库能删什么」。

## 边界

- 只管过度设计和复杂度。正确性 bug、安全、性能不在范围内。
- 只列清单，不动代码。要它动手，你得另外明说。
- 一次性。想退出：说「正常模式」或 `stop ponytail-audit`。

## 致谢

基于 [Ponytail](https://github.com/DietrichGebert/ponytail)（MIT，作者 DietrichGebert）改写，保留了原作者的版权声明（见 `LICENSE`）。

## License

MIT. See [LICENSE](LICENSE).

Made by [公子](https://github.com/chengangrun-ai).
