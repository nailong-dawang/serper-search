# serper-search

[English](#english) | [中文](#中文)

Google search as a CLI via the [Serper API](https://serper.dev) — with automatic multi-key failover.

通过 [Serper API](https://serper.dev) 实现的命令行 Google 搜索，支持多 key 自动故障转移。

---

## <a id="english"></a>English

### Install

```bash
# Requires Python 3.8+, no dependencies (stdlib only)
curl -sL https://raw.githubusercontent.com/nailong-dawang/serper-search/main/bin/serper-search -o serper-search
chmod +x serper-search
```

### Setup

Get API keys at [serper.dev](https://serper.dev) (2,500 free queries). The CLI reads connector IDs from `connectors.txt` (one per line) and tries them in order:

```
custom.serper
custom.serper2
custom.serper3
```

On 400/401/402/403/429 or a missing connector, it silently moves to the next key — a search only fails when **every** key is exhausted. It remembers the last-known-good key to skip dead ones faster.

### Usage

```bash
# Web search (default endpoint)
serper-search "best mechanical keyboards 2026" --num 10

# Other endpoints
serper-search "AI news" --endpoint news --num 5
serper-search "cat" --endpoint images --num 5
serper-search "python tutorial" --endpoint videos --num 5
serper-search "coffee near me" --endpoint places --num 5
serper-search "transformer attention" --endpoint scholar --num 5
serper-search "pyt" --endpoint autocomplete

# Scrape a page (requires Serper scrape plan)
serper-search --endpoint scrape --url "https://example.com/article"
```

### Output

All endpoints print JSON to stdout:

| Endpoint | Fields per result |
|---|---|
| `search` | `{title, url, snippet}` |
| `news` | `{title, url, snippet, date, source}` |
| `images` | `{title, image_url, page_url, thumbnail}` |
| `videos` | `{title, url, thumbnail, duration}` |
| `places` | `{title, address, rating, phone, website}` |
| `scholar` | `{title, url, snippet}` (academic papers) |
| `autocomplete` | `{value}` suggestions |
| `scrape` | `{url, title, text}` (first 8000 chars) |

### Notes

- Keep `--num` modest (5–20); each call costs one Serper credit
- Scrape endpoint requires a Serper scrape plan — free keys return 403
- Auth is restricted to `google.serper.dev` / `scrape.serper.dev` only

---

## <a id="中文"></a>中文

### 安装

```bash
# 需要 Python 3.8+，无第三方依赖（纯标准库）
curl -sL https://raw.githubusercontent.com/nailong-dawang/serper-search/main/bin/serper-search -o serper-search
chmod +x serper-search
```

### 配置

在 [serper.dev](https://serper.dev) 获取 API key（免费 2500 次查询）。CLI 从 `connectors.txt` 按行读取连接器 ID，按顺序尝试：

```
custom.serper
custom.serper2
custom.serper3
```

遇到 400/401/402/403/429 或连接器缺失时自动切换到下一个 key——**所有** key 都用完才算失败。会自动记住上次可用的 key，下次优先用。

### 用法

```bash
# 网页搜索（默认）
serper-search "2026 最好的机械键盘" --num 10

# 其他端点
serper-search "AI 新闻" --endpoint news --num 5        # 新闻
serper-search "猫" --endpoint images --num 5           # 图片
serper-search "python 教程" --endpoint videos --num 5  # 视频
serper-search "附近的咖啡店" --endpoint places --num 5 # 地点
serper-search "transformer attention" --endpoint scholar --num 5  # 学术
serper-search "pyt" --endpoint autocomplete           # 联想词

# 抓取页面正文（需要 Serper scrape 套餐）
serper-search --endpoint scrape --url "https://example.com/article"
```

### 输出

所有端点都向 stdout 输出 JSON：

| 端点 | 每条结果字段 |
|---|---|
| `search` | `{title, url, snippet}` |
| `news` | `{title, url, snippet, date, source}` |
| `images` | `{title, image_url, page_url, thumbnail}` |
| `videos` | `{title, url, thumbnail, duration}` |
| `places` | `{title, address, rating, phone, website}` |
| `scholar` | `{title, url, snippet}`（学术论文） |
| `autocomplete` | `{value}` 联想词 |
| `scrape` | `{url, title, text}`（前8000字符） |

### 说明

- `--num` 别设太大（5–20 即可），每次调用消耗一个 Serper credit
- scrape 端点需要 Serper scrape 套餐，免费 key 会返回 403
- 鉴权请求仅限 `google.serper.dev` / `scrape.serper.dev`

## License

MIT
