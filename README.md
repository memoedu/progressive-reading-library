# 渐进阅读资料库

这里收录渐进阅读工作流采集或从固定来源版本恢复的 Markdown，按现有分类目录组织。可以直接在 GitHub 浏览文件，也可以下载整库 ZIP 或通过 Git 克隆。

带访问令牌等凭据参数的链接已移除相应参数。

订阅工具可通过 `index/articles/` 按来源 URL 查找已归档的文章。索引记录保存原稿的路径和 SHA-256，读者可以从同一个 Git 提交下载原稿并校验内容。

## 通过订阅工具导入

订阅工具优先查资料库。找到原稿时，从对应的 Git 提交下载正文并校验哈希。发现可独立核验的新文章时，同步服务直接提交登录用户导入的原稿，并为匿名用户代提 PR。《渐进阅读工作流》的订阅页可关闭共享同步。具体操作见[订阅共享资料库](https://github.com/memoedu/progressive-reading-workflow/blob/main/docs/SUBSCRIPTION_LIBRARY_SYNC.md)。

- [订阅源索引](index/subscriptions.json)保留公开来源地址。
- [Thoughts Memo 翻译合集](index/repositories/thoughts-memo-translation.json)保留固定的上游提交、目录、文件哈希和许可。读者可从该提交复用原译文。

- [原站目录](%E6%95%B4%E7%AB%99/%E5%8E%9F%E7%AB%99%E7%9B%AE%E5%BD%95.md) 可按原站章节、专题和系列浏览。
