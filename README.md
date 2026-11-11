# 两人笺 · A Journal for Two

> 在这个只剩碎片化联系的时代，亲密的人也不写信了。
> 两人笺是一本只属于两个人的日记：把平日里一闪而过的碎片沉淀下来，不发给谁，不提醒谁——想念的时候，自己来翻。

A private journal for exactly two people. No accounts, no feed, no notifications.
You open it with a shared password, write a few lines or leave a photo,
and the other person reads it whenever they feel like it — like letters, but slower than chat and lighter than mail.

**Live idea:** one web page + one GitHub repository. The code is public (open source);
the content is encrypted before it ever leaves your browser, so a public repository holds nothing but ciphertext.

## 它是什么

- 📖 **一本日记，不是一个网盘**：按日期排布的条目流，两个人轮流写，各有署名和印章色
- 🔐 **密码即钥匙**：内容在浏览器里用 AES-256-GCM 加密，密钥由共同密码经 PBKDF2（310,000 轮）派生；密码不存任何地方
- 🖼 **文字 + 图片**：图片在本地压缩后同样加密保存（视频不在范围内——留在日记里的，应该是字和照片）
- 🤫 **没有通知**：没有推送、没有红点、没有已读。对方想看的时候，自然会来
- 🖨 **可以成册**：内置「打印成册」视图，按时间顺序排版，随时导出 PDF 或打印成一本真正的册子
- 🆓 **零成本、零依赖**：单个 HTML 文件，无框架、无外部资源、无统计无追踪；托管在 GitHub Pages 上

## 安全模型（说清楚，不夸张）

- 仓库里只有三类文件：`index.html`（程序本身）、`vault.json`（只有随机盐，不是秘密）、`config.enc` 与 `entries/*.json`（全部是密文）
- 写日记所需的 GitHub 令牌也被密码加密后存在 `config.enc` 里——只有知道密码的人能解开它来保存新条目
- **没有密码 = 打不开**，包括仓库主人、包括 GitHub。这也意味着：密码忘了，日记就永远锁上了，没有找回功能
- 能被外人看到的只有元数据：文件名里的时间戳（哪天写过），看不到内容、作者和图片
- 因此密码要够长：一句只有你们懂的话，比一串生日数字安全得多

## 部署（约 10 分钟，只需创建者本人操作）

1. 在 GitHub 新建一个**公开**仓库（例如 `two-person-journal`），把本项目的 `index.html` 放进去
   （放心：公开的是程序和密文，不是内容；想更低调也可以用付费版的私有仓库 + Pages，网站本身仍是公开的，所以加密才是真正的锁）
2. 打开仓库 Settings → Pages，选择从 `main` 分支根目录发布，得到网址 `https://<用户名>.github.io/<仓库名>/`
3. 在 GitHub 生成一个**细粒度令牌**（Settings → Developer settings → Fine-grained tokens）：
   只授权这一个仓库，权限只需 **Contents: Read and write**
4. 编辑 `index.html` 顶部的 `JOURNAL_CONFIG`，填入用户名、仓库名，提交
5. 打开网址：第一次打开是「建这本日记」向导——填两个人的署名、设共同密码、粘贴第 3 步的令牌，点创建
6. 把**网址 + 密码**告诉另一个人（最好分开两个渠道发）。对方不需要 GitHub 账号，打开网址输密码即可读写

> 本地试用：不填 `JOURNAL_CONFIG` 直接打开 `index.html`，是单设备演示模式，数据存在本机浏览器里。

## 之后想做的事（Roadmap）

- [ ] 年度成册：按年份自动分卷，生成更精致的书籍版式 PDF
- [ ] 纪念日页：自动把去年的今天翻出来
- [ ] 导出明文备份（输密码后可导出一份属于自己的完整备份）

## License

MIT — 代码随便用，去建你们自己的那一本。
但请记住：工具可以复制，里面的字只属于写字的两个人。
