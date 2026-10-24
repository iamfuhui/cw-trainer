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

## 部署（Cloudflare Pages）

1. 在 GitHub 的 Cloudflare Workers and Pages 应用设置里，确认已授权 `cw-trainer` 仓库。
2. Cloudflare 后台：Workers & Pages → 创建 → Pages → 连接到 Git，选择 `iamfuhui/cw-trainer`。
3. 构建设置：框架预设选"无"，构建命令留空，输出目录填 `/`，保存并部署。
4. 部署完成后，在项目的"自定义域"中添加 `cw.ba8blk.com`，DNS 记录和证书由 Cloudflare 自动处理。

之后推送到 `main` 分支，Cloudflare 会自动重新部署。

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
