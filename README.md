# CW 听抄练习

手机端摩尔斯电码 Koch 听抄练习页，字母顺序与 LCWO 一致。纯静态网页，单个 `index.html`，不需要服务器。

访问地址：https://cw.ba8blk.com

## 功能

- 按 Koch 法分课，第 n 课练前 n+1 个字符，从第 1 课（K M）开始
- 每组 30 秒、1 分钟或 2 分钟连续发送，5 字一组随机生成，新学字母出现频率略高
- 字符速度、有效速度（Farnsworth 字间隔）、音调可调
- 用屏幕下方大按键抄收，支持跳过、退格
- 结束后逐字核对，给出正确率、各字母正确率和易混字母；达到 90% 提示进入下一课
- 日间 / 夜间两种配色
- 成绩保存在本机浏览器，可导出、导入 JSON 备份

## 部署到 GitHub Pages

1. 在 GitHub 新建仓库，例如 `cw-trainer`，把 `index.html`、`CNAME`、`README.md` 三个文件上传到 `main` 分支根目录。
2. 打开仓库 **Settings → Pages**，Source 选 **Deploy from a branch**，分支选 `main`，目录选 `/ (root)`，保存。
3. 在域名服务商的 DNS 里给 `ba8blk.com` 添加一条记录：
   - 类型：`CNAME`
   - 主机记录：`cw`
   - 记录值：`<你的 GitHub 用户名>.github.io`
4. 回到 **Settings → Pages**，Custom domain 填 `cw.ba8blk.com`（`CNAME` 文件已写好，一般会自动填上），等 DNS 检查通过后勾选 **Enforce HTTPS**。

DNS 生效通常需要几分钟到几小时。生效前可以先用 `https://<用户名>.github.io/cw-trainer/` 访问测试。

## 手机使用提示

- **iPhone 静音开关**：打开静音时网页可能没有声音，练习前请关闭静音。
- **添加到主屏幕**：Safari 打开后点"分享 → 添加到主屏幕"，之后可以像 App 一样全屏打开。
- **切到后台会中止本组**：避免声音和计时错位，回来后重新开始即可。
- **备份**：记录只存在当前手机的这个浏览器里。换手机、清理 Safari 数据前，先在"我的档案"里导出备份。

## 档案数据格式（为多人版预留）

导出的 JSON 结构如下，每条记录带呼号字段，以后接服务器时可直接导入：

```json
{
  "version": 1,
  "profile": { "callsign": "BA8BLK" },
  "settings": { "lesson": 1, "duration": 60, "cwpm": 20, "ewpm": 10, "tone": 600, "theme": "dark" },
  "records": [
    {
      "id": "…", "callsign": "BA8BLK", "date": "2026-10-24T08:00:00.000Z",
      "lesson": 2, "duration": 60, "cwpm": 20, "ewpm": 10, "tone": 600,
      "sent": "KMUUK MKUUM", "received": "KMUUK MKUUU",
      "correct": 9, "total": 10, "accuracy": 90,
      "confusions": { "M 听成 U": 1 }
    }
  ]
}
```

存储读写集中在 `index.html` 里的 `Store` 对象。以后改为服务器存储时，只需把 `load`、`save`、`addRecord`、`merge` 换成接口调用，练习和判分部分不用改。
