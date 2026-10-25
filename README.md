# Mihomo Ruleset (MRS)

专为 **Mihomo (Clash.Meta)** 定制的高性能二进制规则集 (`.mrs`) 及纯文本域名列表 (`.txt`)，每日定时全自动同步上游规则、智能去重合并生成。

---

## 规则集与上游来源清单

### 1. 广告拦截 (`ads.mrs` / `ads.txt`)

* **数据来源**：
  * [GOODBYEADS](https://github.com/8680/GOODBYEADS) (`data/rules/dns.txt`)
* **处理说明**：
  * 基于 ABP DNS 语法清洗，剔除修饰符、注释、白名单及纯 IP，提纯为标准的纯域名拦截列表。

---

### 2. AI 平台分流 (`ai.mrs` / `ai.txt`)

整合主流开源社区中维护最活跃的 7 大 AI 规则源，涵盖 OpenAI (ChatGPT)、Anthropic (Claude)、Google Gemini、Microsoft Copilot、Perplexity、Midjourney 等全主流 AI 服务：

* **数据来源**：
  1. [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)（递归解析展开 `category-ai-!cn`）
  2. [ACL4SSR](https://github.com/ACL4SSR/ACL4SSR) (`Clash/Ruleset/AI.list`)
  3. [VPSDance/ai-proxy-rules](https://github.com/VPSDance/ai-proxy-rules) (`rules/clash/global.yaml`)
  4. [blackmatrix7/ios_rule_script - OpenAI](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/OpenAI)
  5. [blackmatrix7/ios_rule_script - Claude](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Claude)
  6. [blackmatrix7/ios_rule_script - Gemini](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/BardAI)
  7. [blackmatrix7/ios_rule_script - Copilot](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Clash/Copilot)
* **处理说明**：
  * **全局去重**：合并所有源的域名规则集合。
  * **上下级智能合并**：自动剔除已被父级域名覆盖的冗余子域名（例如存在 `openai.com` 时自动精简 `chat.openai.com` 等），大幅减小规则集体积并提升内核匹配性能。

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
