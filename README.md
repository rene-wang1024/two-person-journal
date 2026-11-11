# 两人笺 · A Journal for Two

> 在这个只剩碎片化联系的时代，亲密的人也不写信了。
> 两人笺是一本只属于两个人的日记：把平日里一闪而过的碎片沉淀下来，不发给谁，不提醒谁——想念的时候，自己来翻。
>
> *In an age of fragmented contact, even the people closest to us no longer write letters.*
> *A Journal for Two is a diary shared by exactly two people: a place where the fragments of everyday life can settle and keep — sent to no one, reminding no one. You come back and read it when you miss each other.*

A private journal for exactly two people. No accounts, no feed, no notifications.
You open it with a shared password, write a few lines or leave a photo,
and the other person reads it whenever they feel like it — like letters, but slower than chat and lighter than mail.

**How it works:** one web page + one GitHub repository. The code is public (open source);
the content is encrypted before it ever leaves your browser, so a public repository holds nothing but ciphertext.

## 它是什么 · What it is

- 📖 **一本日记，不是一个网盘**：按日期排布的条目流，两个人轮流写，各有署名和印章色
  *A journal, not a file drive: a stream of dated entries, written in turn, each with its author's name and seal colour.*
- 🔐 **密码即钥匙**：内容在浏览器里用 AES-256-GCM 加密，密钥由共同密码经 PBKDF2（310,000 轮）派生；密码不存任何地方
  *The password is the key: content is encrypted in the browser with AES-256-GCM; the key is derived from your shared password via PBKDF2 (310,000 iterations). The password itself is stored nowhere.*
- 🖼 **文字 + 图片**：图片在本地压缩后同样加密保存（视频不在范围内——留在日记里的，应该是字和照片）
  *Text + photos: images are compressed locally and stored encrypted, too. (No video — what belongs in a journal is words and photographs.)*
- 🤫 **没有通知**：没有推送、没有红点、没有已读。对方想看的时候，自然会来
  *No notifications: no pushes, no badges, no read receipts. They will come and read when they want to.*
- 🖨 **可以成册**：内置「打印成册」视图，按时间顺序排版，随时导出 PDF 或打印成一本真正的册子
  *Made to become a book: a built-in print view lays entries out in chronological order, ready to export as PDF or print as a real booklet.*
- 🆓 **零成本、零依赖**：单个 HTML 文件，无框架、无外部资源、无统计无追踪；托管在 GitHub Pages 上
  *Zero cost, zero dependencies: a single HTML file — no frameworks, no external resources, no analytics, no tracking — hosted on GitHub Pages.*

## 安全模型（说清楚，不夸张）· Security model, stated plainly

- 仓库里只有三类文件：`index.html`（程序本身）、`vault.json`（只有随机盐，不是秘密）、`config.enc` 与 `entries/*.json`（全部是密文）
  *The repository holds only three kinds of files: `index.html` (the app), `vault.json` (a random salt — not a secret), and `config.enc` / `entries/*.json` (all ciphertext).*
- 写日记所需的 GitHub 令牌也被密码加密后存在 `config.enc` 里——只有知道密码的人能解开它来保存新条目
  *The GitHub token needed to save entries is itself encrypted with the password inside `config.enc` — only someone who knows the password can unlock it and write.*
- **没有密码 = 打不开**，包括仓库主人、包括 GitHub。这也意味着：密码忘了，日记就永远锁上了，没有找回功能
  *No password = no access — not for the repo owner, not for GitHub. Which also means: lose the password and the journal stays locked forever. There is no recovery.*
- 能被外人看到的只有元数据：文件名里的时间戳（哪天写过），看不到内容、作者和图片
  *The only thing visible to outsiders is metadata: the timestamps in file names (which days have entries). Not content, not authors, not photos.*
- 因此密码要够长：一句只有你们懂的话，比一串生日数字安全得多
  *So make the password long: a sentence only the two of you understand beats a string of birthdays.*

## 部署（约 10 分钟）· Deploy in about 10 minutes

1. 在 GitHub 新建一个**公开**仓库，把本项目的 `index.html` 放进去
   *Create a **public** GitHub repository and put this project's `index.html` in it.*
   （放心：公开的是程序和密文，不是内容。）
   *(Don't worry: what is public is the code and the ciphertext — never the content.)*
2. 打开仓库 Settings → Pages，选择从 `main` 分支根目录发布，得到网址 `https://<用户名>.github.io/<仓库名>/`
   *In the repo's Settings → Pages, publish from the `main` branch root. Your site will be at `https://<username>.github.io/<repo>/`.*
3. 在 GitHub 生成一个**细粒度令牌**（Settings → Developer settings → Fine-grained tokens）：只授权这一个仓库，权限只需 **Contents: Read and write**
   *Create a **fine-grained token** (Settings → Developer settings → Fine-grained tokens) scoped to this repository only, with just **Contents: Read and write**.*
4. 编辑 `index.html` 顶部的 `JOURNAL_CONFIG`，填入用户名、仓库名，提交
   *Edit `JOURNAL_CONFIG` at the top of `index.html` with your username and repo name, and commit.*
5. 打开网址：第一次打开是「建这本日记」向导——填两个人的署名、设共同密码、粘贴第 3 步的令牌，点创建
   *Open the site. The first visit shows a setup wizard: enter the two authors' names, set your shared password, paste the token from step 3, and create the journal.*
6. 把**网址 + 密码**告诉另一个人（最好分开两个渠道发）。对方不需要 GitHub 账号，打开网址输密码即可读写
   *Tell the other person the **URL + password** (ideally via two separate channels). They need no GitHub account: open the URL, enter the password, read and write.*

> 本地试用：不填 `JOURNAL_CONFIG` 直接打开 `index.html`，是单设备演示模式，数据存在本机浏览器里。
> *Local try-out: open `index.html` without filling in `JOURNAL_CONFIG` — a single-device demo mode stored in your browser.*

## 两条保命须知 · Two rules that protect the journal

- **永远不要删除仓库里的 `vault.json` 和 `config.enc`**：盐在前者里，删了等于把钥匙熔了，全部日记永久打不开（删日记条目没关系，删这两个文件不行）
  ***Never delete `vault.json` or `config.enc` from the repository.** The salt lives in the first; deleting it melts the key and locks the whole journal forever. (Deleting entries is fine. Deleting these two files is not.)*
- **令牌会过期**（细粒度令牌最长约一年）：到期前在日记页右上角「设置」里粘贴新令牌即可无缝续上，密码和已有日记都不受影响
  ***Tokens expire** (fine-grained tokens last about a year at most). Before it does, paste a fresh token under "设置 / Settings" in the journal's top corner — the password and all existing entries carry on untouched.*

## 之后想做的事 · Roadmap

- [ ] 年度成册：按年份自动分卷，生成更精致的书籍版式 PDF
  *Yearly volumes: entries grouped by year into a finer book-layout PDF.*
- [ ] 纪念日页：自动把去年的今天翻出来
  *On-this-day: last year's page, surfaced on its anniversary.*
- [ ] 导出明文备份（输密码后可导出一份属于自己的完整备份）
  *Plain-text export: after unlocking, download a complete backup of your own.*

## License

MIT — 代码随便用，去建你们自己的那一本。
但请记住：工具可以复制，里面的字只属于写字的两个人。

*MIT — take the code and build your own.*
*But remember: the tool can be copied; the words inside belong only to the two people who wrote them.*
