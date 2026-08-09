# Athlete Sleep Check｜Q6 GitHub Pages 发布包

本文件夹已经预先写入正式 Google Apps Script /exec 地址，无需再修改代码。

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
