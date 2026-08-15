# Athlete Sleep Check｜Q6 部署与本地引擎

`config.js`保留现有正式 Google Apps Script `/exec` 地址，GitHub Pages 仍使用原仓库与原网址：

`https://houutou.github.io/athlete-sleep-check-q6/`

因此原二维码无需重做。升级到 1.3.4 时，必须先把配套 `Code.gs` 更新到**同一个 Apps Script 项目、同一个现有 Web App 部署**，确认 `/exec` 仍是原地址后，再覆盖公开仓库中的六个文件。不要新建仓库，也不要新建另一个 Web App deployment。

1.3.4 在不改变六道题、字段、公开网址或 `/exec` 地址的前提下，为等待确认的提交增加端侧临时保留。只有收到匹配的 `accepted=true` 和 `record_id` 后才清除；网络、超时或后台异常时保留72小时，供用户使用同一受付番号重试。

Backend 1.3.4 将服务端数据即时写入 `RAW_SUBMISSIONS` 并强制刷新后，再处理 `DAILY_ANALYSIS`。后台每3小时执行一次幂等补漏与条件汇总；每天凌晨4点执行完整一致性检查。这些云端任务不依赖本地Mac开机。

Backend 不要求预先建立运动员名册。格式合法的非 `TEST*` ID 可直接提交；`ATHLETE_ROSTER` 仅用于以后补充分组、明确停用某个 ID 或设置有效日期窗口。

## 上传到 GitHub 的文件

公开仓库根目录严格只保留以下六个文件：

- index.html
- config.js
- manifest.webmanifest
- icon.svg
- sw.js
- README_CN.md

不得上传 Apps Script 后台源码、管理后台、数据库、CSV、备份、报告或运动员资料。

## GitHub Pages 设置

1. 打开仓库的 Settings。
2. 左侧进入 Pages。
3. 在 Build and deployment 下：
   - Source：选择 Deploy from a branch
   - Branch：选择 main
   - 文件夹：选择 /(root)
4. 点击 Save。
5. 等待约1～5分钟，刷新 Pages 页面，确认仍为原公开网址。

## 正式使用检查

Backend 更新后检查：

- `/exec` 返回 `backend_version: 1.3.4`；
- `SYNC_STATUS` 为 `PASS`；
- `unsynced_pending` 和 `unsynced_failed` 均为0；
- Apps Script 只保留 `threeHourlyMaintenance` 和 `dailyFullConsistencyCheck` 两个治理触发器；
- 原公开网址和二维码继续使用。

不要为了验证而新增正式测试记录。

## 管理可视化

高级管理可视化位于私有本地引擎，不在公开仓库中。当前采用受治理的 `DAILY_ANALYSIS.csv` 导入模式；公开GitHub Pages不保存运动员数据，也不直接公开Spreadsheet读取接口。

## 私有后台边界

`q6_engine/`、`q6_console.py`、`admin/`和`.command`维护入口属于私有后台代码，不应部署到公开GitHub Pages。真实SQLite、备份和导出文件必须继续排除在Git之外。

本地 v0.4.0 常用入口：

- `VERIFY_Q6.command`：检查当前数据库完整性。
- `START_Q6.command`：启动本地管理后台。
- `BACKUP_Q6.command`：创建快照并立即完成临时恢复验证。
- `RESTORE_TEST_Q6.command`：重新验证最新备份能否恢复。
- `TEST_Q6.command`：运行Python、HTTP、前端与容量回归测试。

正式导入只接受精确命名的 `DAILY_ANALYSIS.csv` 或 `DAILY_ANALYSIS.json`。编号相似表（例如 `02_CURRENT_DAILY`）不得进入正式引擎。
