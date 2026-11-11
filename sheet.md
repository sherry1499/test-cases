# 飞书用例表

## 列

| 列 | 字段 | 对齐 |
|---|---|---|
| A | 用例编号 | 居中 |
| B | 优先级 | 居中。P0 加粗，其余不加粗 |
| C | 用例类型 | 居中。功能 / 兼容 / 回归 / 边界 |
| D | 所属端 | 居中 |
| E | 一级功能 | 居中 |
| F | 二级功能 | 居中 |
| G | 功能模块 | 居中 |
| H | 用例标题 | 居中 |
| I | 前置条件 | 左对齐，自动换行 |
| J | 测试步骤 | 左对齐，自动换行 |
| K | 测试数据 | 左对齐，自动换行 |
| L | 预期结果 | 左对齐，自动换行 |
| M | 执行结果 | 居中。下拉：待执行、通过、失败、阻塞、不适用 |
| N | 执行人 | 居中 |
| O | 执行日期 | 居中 |
| P | 备注 | 左对齐 |

表头：背景 `#1F4E79`，白字，微软雅黑 10 加粗，行高 32。正文：微软雅黑 8，数字格式 `@`，垂直居中，背景白，边框 `#D0D7DE`。冻结首行首列。数据行行高用 auto。

列宽（像素）：A110 B56 C56 D88 E88 F88 G88 H240 I200 J240 K140 L280 M80 N72 O90 P140。

## 调用方式

Windows 上用 `py -3`。不要把带中文的 JSON 交给 PowerShell，引号会被吃掉。用 Python 把 UTF-8 JSON 从 stdin 传给：

`C:\Users\14998\AppData\Roaming\npm\node_modules\@larksuite\cli\scripts\run.js`

身份始终 `--as user`。表格用 `--spreadsheet-token` 或知识库 URL。知识库 URL 会先解析节点，解析超时时改用表格 token。

- 新建知识库表格：`wiki +node-create --parent-node-token <需求父节点> --obj-type sheet --title "..."`。
- 写入：`sheets +cells-set --writes -`。每批大约 8 行。单元格只传 `value` 时保留原样式。
- 插行：`sheets +dim-insert --position <行号> --count N --inherit-style before`。新行插在该行号之前。从下往上插，避免先插入的行号被后面的插入顶偏。
- 删行：`sheets +dim-delete --range 起:止 --yes`。先确认这些行确实是要去掉的重复用例。
- 下拉：`sheets +dropdown-update --ranges '["功能用例!M2:M{末行}"]' --options '["待执行","通过","失败","阻塞","不适用"]'`。
- 冻结：`sheets +dim-freeze --rows 1 --cols 1`。
- 改过的行再 `+rows-resize --type auto`。

不要用 `+batch-update --yes`。先改对齐再给 B 列上加粗时，最后再把 P0 的 B 列设回加粗，避免被整段样式盖掉。

写完用 `+cells-get` 读回编号、标题和相邻行。接口超时时先读回，确认已经写上就不要原样重发。
