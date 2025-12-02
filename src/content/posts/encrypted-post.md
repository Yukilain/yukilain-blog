---
title: 加密文章示例
published: 2024-01-15
description: 这是一篇用于测试页面加密功能的文章
encrypted: true
pinned: true
password: "123456"
permalink: "encrypted-example"
tags: ["测试", "加密"]
category: "技术"
draft: true
---

这个博客模板是基于 [Astro](https://astro.build/) 构建的。对于本指南中未提及的内容，您可以在 [Astro 文档](https://docs.astro.build/) 中找到答案。

## 文章的 Front-matter

```yaml
---
title: 我的第一篇博客文章
published: 2023-09-09
description: 这是我新 Astro 博客的第一篇文章。
image: ./cover.jpg
tags: [示例, 教程]
category: 前端
draft: false
---
```




| 属性          | 描述                                                                                                                                                                                                            |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `title`       | 文章的标题。                                                                                                                                                                                                      |
| `published`   | 文章发布日期。                                                                                                                                                                                                    |
| `pinned`      | 文章是否置顶显示在文章列表顶部。                                                                                                                                                                                  |
| `description` | 文章的简短描述，显示在索引页上。                                                                                                                                                                                  |
| `image`       | 文章的封面图片路径。<br/>1. 以 `http://` 或 `https://` 开头：使用网络图片<br/>2. 以 `/` 开头：引用 `public` 目录下的图片<br/>3. 无上述前缀：相对于当前 Markdown 文件的路径                                        |
| `tags`        | 文章的标签。                                                                                                                                                                                                      |
| `category`    | 文章的分类。                                                                                                                                                                                                      |
| `permalink`   | 文章的自定义永久链接。文章将可通过 `/posts/{permalink}/` 访问。例如：`my-special-article`（将可通过 `/posts/my-special-article/` 访问）                                                                        |
| `licenseName` | 文章内容的许可证名称。                                                                                                                                                                                            |
| `author`      | 文章的作者。                                                                                                                                                                                                      |
| `sourceLink`  | 文章内容的来源链接或参考资料。                                                                                                                                                                                    |
| `draft`       | 如果文章仍为草稿状态，则不会显示。                                                                                                                                                                                |

## 文章文件存放位置



您的文章文件应该放在 `src/content/posts/` 目录下。您也可以创建子目录来更好地组织您的文章和资源。

```
src/content/posts/
├── post-1.md
└── post-2/
    ├── cover.png
    └── index.md
```

## 自定义永久链接

您可以通过在 front-matter 中添加 `permalink` 字段来为任何文章设置自定义永久链接：

```yaml
---
title: 我的特别文章
published: 2024-01-15
permalink: "my-special-article"
tags: ["示例"]
category: "技术"
---
```

设置自定义永久链接后：
- 文章将可通过自定义 URL 访问（例如，`/posts/my-special-article/`）
- 默认的 `/posts/{slug}/` URL 仍然有效
- RSS/Atom 订阅源将使用自定义永久链接
- 所有内部链接将自动使用自定义永久链接

**重要提示：**
- 永久链接不应包含 `/posts/` 前缀（它会自动添加）
- 避免在永久链接中使用特殊字符和空格
- 为获得最佳 SEO 效果，请使用小写字母和连字符
- 确保所有文章的永久链接唯一
- 不要包含前导或尾随斜杠


## 工作原理

```mermaid
graph LR
    A[用户密码] --> B[bcrypt 哈希]
    B --> C[密码哈希值]
    C --> D[提取前32个字符]
    D --> E[加密密钥]
    E --> F[AES 加密]
    F --> G[加密内容]
```
