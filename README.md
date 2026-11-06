# Mihomo Ruleset (MRS)

专为 **Mihomo (Clash.Meta)** 定制的高性能二进制规则集 (`.mrs`) 及纯文本域名列表 (`.txt`)，每日定时全自动同步上游权威规则、智能清洗去重合并生成。

---

## 规则集与上游来源清单

### 1. 广告拦截 (`ads.mrs` / `ads.txt`)

多源聚合互补，全面拦截全网广告、开屏广告、追踪器与流氓 SDK：

* **数据来源**：
  1. [GOODBYEADS](https://github.com/8680/GOODBYEADS) (`data/rules/dns.txt`)
  2. [anti-AD](https://github.com/privacy-protection-tools/anti-AD) (`anti-ad-domains.txt`)
  3. [AWAvenue-Ads-Rule](https://github.com/TG-Twilight/AWAvenue-Ads-Rule) (`AWAvenue-Ads-Rule-Clash.yaml`)
* **处理说明**：
  * 清洗过滤 ABP 语法修饰符、注释、白名单及纯 IP，剔除畸变通配符，聚合去重为标准的纯域名拦截列表。

---

### 2. AI 平台分流 (`ai.mrs` / `ai.txt`)

整合主流开源社区中维护最活跃的权威 AI 规则源，全面覆盖主流生成式 AI 与开发者工具：

* **涵盖服务**：
  * **Anthropic / Claude / Claude Code** (claude.ai, anthropic.com, claude.com, claude.site 等全系列及开发工具)
  * **OpenAI** (ChatGPT, Sora, OpenAI API)
  * **Google Gemini / DeepMind** (Gemini, AI Studio, Bard, NotebookLM)
  * **Microsoft Copilot** (Github Copilot, Windows Copilot)
  * **xAI (Grok)** (grok.com, x.ai, grokipedia.com)
  * **Perplexity AI** (perplexity.ai, pplx.ai)
  * **Groq / Midjourney / Poe / Windsurf / Cursor / Minimax** 等
* **数据来源**：
  1. [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)（递归展开 `category-ai-!cn`）
  2. [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) (`Clash/Ruleset/AI.list`)
  3. [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules) (`rules/clash/global.yaml`)
  4. [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script) (OpenAI / Claude / Anthropic / Gemini / Copilot)
  5. [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat) (Perplexity / xAI / Groq / Anthropic)
* **处理说明**：
  * **全局去重**：合并所有源的域名规则集合。
  * **上下级智能合并**：自动剔除已被父级域名覆盖的冗余子域名（例如存在 `openai.com` 时自动精简 `chat.openai.com` 等），大幅精简规则体积，保证极速匹配。

---

## 订阅配置示例

在 Mihomo (Clash.Meta) 的 `config.yaml` 中配置：

```yaml
rule-providers:
  ads:
    type: http
    behavior: domain
    format: mrs
    path: ./ruleset/ads.mrs
    url: "https://raw.githubusercontent.com/LinkProxy-Core/mihomo-mrs/main/ads.mrs"
    interval: 86400

  ai:
    type: http
    behavior: domain
    format: mrs
    path: ./ruleset/ai.mrs
    url: "https://raw.githubusercontent.com/LinkProxy-Core/mihomo-mrs/main/ai.mrs"
    interval: 86400

rules:
  - RULE-SET,ads,REJECT
  - RULE-SET,ai,AI 平台
  # 其他分流规则...
  - MATCH,漏网之鱼
```
