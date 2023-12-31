# Report 报表 (WIP)

## Domain Entities

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
  - id: uuid
    text: string
    values:
    - key: string # key of the column
      value: number # value of the cell
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
      values:
      - key: string # key of the column
        value: number # value of the column for current formula
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

`Report` 会和 `Template` 公用数据结构, 在此之上, `Report` 多了账期和版本信息.

``` go
type Report struct {
    periodId uuid.UUID
    version  int
    Template
}
```
