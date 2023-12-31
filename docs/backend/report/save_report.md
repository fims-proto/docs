# When to Save Report

1. 当前账期关闭的时候, 会保存 (更新) 一次当前账期的 report. See [Close Period](../general_ledger/close_period.md).
2. 在那之前, 每次用户查看 report 时, 针对查看的账期 (可能是当前账期, 也有可能是未来账期), 如果还没有 report 生成时, 应该在此时生成.

    > 当创建 voucher 时, 可以创建在未来, 这时候会生成未来的账期.

3. 用户在查看 report 的时候, 应该有机会要求重新生成 report.
4. 过去的账期应该都有 report, 除非是新增了 template. 这种情况应该可以通过第 3 点解决.

## 建议的 UI

当前端尝试渲染 report, 请求后端 find report, 但是拿到了 404 时, 前段提示 ***Report not yet generated***. 并且提供按钮 `生成报表`, 触发后端的根据 `templateId` + `periodId` 调用 `processor` 生成 report 并保存. 前端知晓 202 created 之后, 再次调用 find report API.

即, 用户流程中, 只有两个点会触发后端计算并保存 (或更新) report:

1. `生成报表` 按钮
1. `Close Period` 流程

## 建议的后端

- Database 中, 不保存 `template` 里面的 values
- 每次编辑 `report` 时其实显示的是**当前 period** 的 report, 而保存时调用的是 `template` 的更新接口
