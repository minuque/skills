---
name: ponytail-review
description: 过度设计审查：只查臃肿与 YAGNI，不改代码。用户调用或 /skill:ponytail-review 时使用。
disable-model-invocation: true
---

只查过度设计。不改代码。正确性、安全、性能不在范围。冒烟或一条 assert 自检不算臃肿。

无事可砍时只回：已经够瘦，可发。

## 分类

按类分组，类名写成「中文 (english)」：

* 删除 (delete)：死代码、未用灵活性、臆测功能

* 标准库 (stdlib)：手写了标准库已有能力

* 原生 (native)：依赖或代码重复平台能力

* 过度设计 (yagni)：单实现抽象、无人改的配置、单调用方分层

* 压缩 (shrink)：逻辑不变、更短

没有该类就省略标题。

## 格式

默认列表。每条：

* `文件:L行` 问题

  * 建议：替换；删除类写「删」

结构、调用、职责边界用 show-me 最小图（调用树、文件树、Mermaid、diff 草图），紧挨对应列表。列表能说清就不要图。

末行：`净可减：-N 行`

## 示例

### 标准库 (stdlib)

* `email.ts:L12-38` 27 行校验类

  * 建议：`"@" in email`，真校验靠确认邮件

### 原生 (native)

* `time.ts:L4` 为一次格式化引入 moment.js

  * 建议：`Intl.DateTimeFormat`，0 依赖

### 过度设计 (yagni)

* `repo.py:L88` AbstractRepository 只有一个实现

  * 建议：内联，直到出现第二个实现

```text
save()
  AbstractRepository.save
    SqlRepository.save
```

改为：

```text
save()
  SqlRepository.save
```

### 删除 (delete)

* `retry.ts:L52-71` 幂等本地调用外包一层重试

  * 建议：删

### 压缩 (shrink)

* `group.ts:L30-44` 手写循环拼 dict

  * 建议：`dict(zip(keys, values))`
