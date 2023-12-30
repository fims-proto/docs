# Report 报表 (WIP)

## Concept and sample

### Balance Sheet 资产负债表

`资产 = 负债 + 所有者权益`

|资产|期末余额|年初余额|负债和所有者权益|期末余额|年初余额|
|-|-|-|-|-|-|
|流动资产|||流动负债|||
|...|||...|||
|流动资产合计|123.00|123.00|流动负债合计|123.00|123.00|
|非流动资产|||非流动负债|||
|...|||...|||
|非流动资产合计|123.00|123.00|非流动负债合计|123.00|123.00|
|&nbsp;||||||
||||所有者权益|||
||||...|||
||||所有者权益合计|123.00|123.00|
|资产总计|123.00|123.00|负债和所有者权益总计|123.00|123.00|

### Income Statement 利润表

营业利润 = 营业收入 - 营业成本 - 销售费用 - ...
利润总和 = 营业利润 + 营业外收入 - 营业外支出
净利润 = 利润总和 - 所得税费用
(综合收益总额 = 净利润 + 其他综合收益的税后净额)

|项目|本年累计金额|本月金额|
|-|-|-|
|1. 营业收入|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;减: 营业成本|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;销售费用|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;管理费用|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...|...|...|
|2. 营业利润（亏损以“-”号填列）|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;加: 营业外收入|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;减: 营业外支出|123.00|123.00|
|3. 利润总额（亏损总额以“-”号填列）|123.00|123.00|
|&nbsp;&nbsp;&nbsp;&nbsp;减: 所得税费用|123.00|123.00|
|3. 净利润（净亏损以“-”号填列）|123.00|123.00|

### Others

另外还有一些标准报表: 现金流量表, 主要应交税金明细表, 费用明细表. 之后再说.

## Design

### Domain Entities

**Template**:

``` yaml
id: uuid
sobId: uuid
class: enum # 报表类别, 资产负债表|利润表|etc.
title: string
tables:
- header:
    text: string
    columns:
    - key: string # key of the column
      value: string # title of the column
  items:
  - values:
    - key: string # key of the column
      value: number # value of the cell
    id: uuid
    text: string
    level: int
    sumFactor: int # 1/-1/0, 当前 item 是被加(1)还是减(-1)在合计里面. 0 代表在合计里面忽略，有这几种情况:
    # - item 是特殊行（比如标题）没有数字值
    # - breakdown item，作为上级数字的细分
    # - item 是合计行, 或阶段性的合计行（dataSource 是 sum）
    rowNumber: int
    sequence: int
    dataSource: enum # formula/sum/na, 当前 item 的数据源是来自于公式(formula), 还是截至目前的累加(sum)
    formulas: # 当 dataSource 是 `formula` 时，此字段将包含一个或多个 formula:
    - accountId: string
      sumFactor: int # 1/-1, 当前科目数字`加`或`减`
      rule: # 取数规则，依据 report type 不同，有两套不同的取数规则:
      # - 资产负债表 B/D/C/AD/AC/CAD/CAC:
      #   - B: balance, 余额
      #   - DB: debit balance, 借方余额
      #   - CB: credit balance, 贷方余额
      #   - ADB ?: account debit balance, 科目借方余额
      #   - ACB ?: account credit balance, 科目贷方余额
      #   - CADB ?: current account debit balance, 当前科目借方余额
      #   - CACB ?: current account credit balance, 当前科目贷方余额
      # - 利润表 PL/D/C:
      #   - PL: profit and loss, 损益发生额
      #   - DA: debit amount, 借方发生额
      #   - CA: credit amount, 贷方发生额
    displaySumFactor: bool
    displayRowNumber: bool
    isBreakdownItem: bool
    isDeletable: bool
    isNameModifiable: bool
    isDraggable: bool
    isAbleToAddChild: bool
    isAbleToAddSibling: bool
    isAbleToAddLeaf: bool
```

**Report**:

基本上是 `template` 的子集, 去掉了一些 design time 的属性, 仅保留了为支持 display 而存在的属性:

``` yaml
---
id: uuid
sobId: uuid
yearPeriod: string # fiscal year + period number, e.g. 202302
class: enum
title: string
version: int
tables:
- header:
    text: string
    columns:
    - key: string # key of the column
      value: string # title of the column
  items:
  - values:
    - key: string # key of the column
      value: number # value of the cell
    id: uuid
    text: string
    level: int
    rowNumber: int
    sequence: int
    displaySumFactor: bool
    displayRowNumber: bool
    isBreakdownItem: bool
```
