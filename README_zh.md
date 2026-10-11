# serper-search

通过 [Serper API](https://serper.dev) 使用 Google 搜索 —— 带多 Key 自动切换的命令行工具。

[English](README.md)

## 安装

```bash
# 去 https://serper.dev 免费获取 API Key（每月 2500 次免费）
export SERPER_API_KEY="你的key"
```

## 用法

```bash
./bin/serper-search "关键词" [--num 10] [--endpoint search|news|images|videos|places|scholar|autocomplete]
./bin/serper-search --endpoint scrape --url "https://example.com/page"
```

- `search` / `news`：返回 `{title, url, snippet}` JSON 数组（news 还多 `date`、`source`）
- `images`：`{title, image_url, page_url, thumbnail}`
- `videos`：`{title, url, thumbnail, duration}`
- `places`：`{title, address, rating, phone, website}`
- `scholar`：学术论文搜索
- `autocomplete`：`{value}` 搜索建议
- `scrape`：`{url, title, text}`（前 8000 字符，需要 scrape 套餐）

## 多 Key 自动切换

设置 `SERPER_API_KEY` 环境变量（多个 key 用逗号分隔），或把 key 一行一个写进 `connectors.txt`。遇到 401/402/403/429 会自动换下一个 key，只有所有 key 都用完才报错。

## 许可证

MIT
