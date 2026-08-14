# Athlete Sleep Check｜Q6 部署与本地引擎

`config.js`保留正式 Google Apps Script `/exec` 地址。公开问卷升级到1.3.1前，必须先把`google_apps_script/Code.gs`部署到同一正式Spreadsheet，并完成验收；不得只升级网页。

Backend 1.3.1 不要求预先建立运动员名册。格式合法的非 `TEST*` ID 可直接提交；`ATHLETE_ROSTER` 仅用于以后补充分组、明确停用某个 ID 或设置有效日期窗口。

## 上传到 GitHub 的文件

把以下五个文件放在仓库最外层：

- index.html
- config.js
- manifest.webmanifest
- icon.svg
- sw.js

README_CN.md 是说明文件，可一起上传，也可以不上传。

## GitHub Pages 设置

1. 打开仓库的 Settings。
2. 左侧进入 Pages。
3. 在 Build and deployment 下：
   - Source：选择 Deploy from a branch
   - Branch：选择 main
   - 文件夹：选择 /(root)
4. 点击 Save。
5. 等待约1～5分钟，刷新 Pages 页面，复制显示的公开网址。

## 正式使用前

先实际提交一条测试记录，然后确认 Google Spreadsheet：

- RAW_SUBMISSIONS 新增1行；
- DAILY_ANALYSIS 新增或更新1行；
- SYNC_STATUS 的 last_record_id 和 last_received_at 已更新；
- 测试记录的 submission_status 为 ACCEPTED。

确认完成后，再用 GitHub Pages 的公开网址生成二维码。

## 管理可视化（配置中）

admin/ 包含隔离的 Q6 Sleep Intelligence Console。当前采用本地导入模式：数据只在管理员浏览器内存中处理，公开仓库不保存运动员数据，也不直接公开 Spreadsheet 读取接口。

- 管理端说明：admin/README_CN.md
- 数据契约：admin/DATA_CONTRACT.md
- 正式问卷页面与 Apps Script 提交链路保持不变。

## 私有后台仓库

`q6_engine/`、`q6_console.py`、`admin/`和`.command`维护入口属于私有后台代码，不应部署到公开GitHub Pages。真实SQLite、备份和导出文件已由`.gitignore`排除。

v0.4.0常用入口：

- `VERIFY_Q6.command`：检查当前数据库完整性。
- `START_Q6.command`：启动本地管理后台。
- `BACKUP_Q6.command`：创建快照并立即完成临时恢复验证。
- `RESTORE_TEST_Q6.command`：重新验证最新备份能否恢复。
- `TEST_Q6.command`：无需npm，运行Python单元、HTTP和10,000人/100,000行容量回归。

v0.4.0增加：静态文件白名单、仅本机随机会话、受控图表范围、四类正式报告、研究冻结ZIP、QA处理说明与闭环审计。完整发布顺序见`DEPLOY_Q6_v0_4_0_CN.md`。

正式导入只接受精确命名的`DAILY_ANALYSIS.csv`或`DAILY_ANALYSIS.json`。编号相似表（例如`02_CURRENT_DAILY`）不得进入正式引擎。
