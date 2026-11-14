# CFNB ProxyIP V5–V7 交付验收记录

更新时间：2026-11-14，Asia/Shanghai  
分支性质：**仅实验 / staging，不代表生产发布**。

## 验收结论

经过44组来源已筛选 ProxyIP 初筛、32 MiB双下载源晋级、100 MiB重复压力测试，以及同浏览器环境 Net.Coffee ChatGPT/Claude 风控参考，当前可推荐**独立试用**的组合为：

- **Cloudflare EdgeTunnel Worker：US 模板**（不改变原有域名与 UUID）
- **入口 IP：`46.8.227.89:8443`**
- **路径级 PROXYIP：`170.9.4.100:443`**
- **实际出口（Net.Coffee ChatGPT/Claude）：`163.192.4.245`，US Oracle Cloud**
- **4×100 MiB：4/4 完整，端到端速度中位83.83 Mbps、最低80.48 Mbps；无 curl 56**
- **未授权 OpenAI API 基础连接 TTFB 中位：1156.9 ms**（不是实际 Codex 推理性能）
- **Net.Coffee Trust Score：ChatGPT 77/100，Claude 77/100**
- **本轮未修改正在使用的 v2rayN 活动节点、TUN、Cloudflare KV、GitHub 正式订阅或 Worker**。

## 为什么没有采用单纯最快的 PROXYIP

### V5，2026-11-14

固定入口 `141.98.41.87:443`、US EdgeTunnel，44组候选测试完毕，9组完成晋级阶段：

| ProxyIP | 32MiB双源中位Mbps | 完整 | API TTFB ms |
|---|---:|---:|---:|
| proxyip.cmliussss.net | 60.34 | 2/2 | 2465 |
| 170.9.4.100 | 60.14 | 2/2 | 1320 |
| 163.192.62.250 | 59.33 | 2/2 | 1376 |
| 原US反代 | 57.91 | 2/2 | 2537 |
| 168.138.165.174 | 57.03 | 2/2 | 2011 |
| 192.9.136.45 | 56.39 | 2/2 | 1269 |

报告：`cfnb-proxyip-v5-curated-20261114-114250/RESULTS.json`

### V6，2026-11-14

仍使用入口141.98.41.87，4个路径进行100MiB交叉重复测试：

- 原US反代：1/4 完整。
- 170.9.4.100：2/4 完整，部分测试达到92–97Mbps，但第二轮连续断流。
- 192.9.136.45：2/4 完整。
- 163.192.62.250：成功完整1次、另一次出口识别失败，不能算稳定。

因此**不能直接发布基于141.98.41.87的高速版本**。  
报告：`cfnb-v6-proxyip-abba-100m-20261114-115355/RESULTS.json`

### V7，2026-11-14

改为固定PROXYIP 170.9.4.100，采用同一 US Worker 和Xray，比较入口：

| 入口 | 100MiB完整率 | Mbps中位数 | 最低完整下载Mbps | API基础TTFB |
|---|---:|---:|---:|---:|
| **46.8.227.89:8443** | **4/4** | **83.83** | **80.48** | **1157ms** |
| 141.98.41.87:443 | 3/4 | 94.05（失败样本按0计后取得） | 发生1次断流 | 1366ms |
| 149.248.32.207:443 | 3/4 | 82.26（失败样本按0计） | 发生1次断流 | 1776ms |

中位数对失败样本按0计；`141.98.41.87`仍有一次失败，不因其他几次高峰而晋升。  
报告：`cfnb-v7-ingress-swap-20261114-120352/RESULTS.json`

## IP信任参考：同入口，同隔离Edge浏览器

Net.Coffee是第三方启发式评分，**不是 OpenAI 或 Anthropic 内部风控评分**。每条路径在独立、无登录状态、相同浏览器参数的无头 Edge 中检测。

| 路径（入口46.8.227.89） | ChatGPT | Claude | 检测到的出口 |
|---|---:|---:|---|
| 原US PROXYIP | 84 | 80 | GPT: 203.88.123.113（澳大利亚）；Claude: 141.98.41.28（美国） |
| **170.9.4.100** | **77** | **77** | 两站均为 163.192.4.245（美国Oracle Cloud） |
| 无路径覆盖 | 46 | 44 | Cloudflare WARP共享出口104.28.* |

- 无路径覆盖产生的 WARP 出口信誉较低，与用户此前的评分观察相符。
- 原US反代虽然评分更高，但 API 基础响应和100MiB稳定性在本轮较差，且对不同目标的出口 IP 不同。
- 新组合在两站表现更一致，但仍是IDC/Hosting；浏览器时区、语言与地理位置存在不匹配。信任分并非长期保证。
- ProxyCheck把 Oracle 与 Cloudflare 相关地址都标成VPN、风险66，无法有效区分该次结果；不能代替Net.Coffee。
- 仍需真实登录状态下的Codex/ChatGPT日常使用验收及跨日复测。

报告：
- `cfnb-netcoffee-headless-20261114-120828/RESULTS.json`（新路径GPT）
- `cfnb-netcoffee-headless-20261114-121110/RESULTS.json`（旧/无覆盖GPT）
- `cfnb-netcoffee-headless-20261114-121318/RESULTS.json`（Claude）
- `cfnb-v6-egress-risk-20261114-115938/RESULTS.json`（风险API）

## 本地可导入试验配置和回退

- 试验 VLESS 文件：`C:\Tools\cfnb-quality\node-pool-rebuild-20261109\CFNB_V7_FAST_TRUST77_IMPORT_20261114.txt`
- 验收清单：`C:\Tools\cfnb-quality\node-pool-rebuild-20261109\CFNB_V7_FAST_TRUST77_MANIFEST_20261114.json`
- **VLESS URL/UUID 不进入此公共GitHub分支**。
- 生产US/SG订阅维持原状，当前活动节点未切换。
- 推荐先在v2rayN中作为独立试验项导入，不覆盖原订阅；只在日常业务体验验证后再讨论生产发布。

## 方法与证据边界

独立Xray、物理网卡绑定、Cloudflare Trace出口检测、SocketWitnessSampler见证、HTTP 206与完整字节计数作为技术证据。未经授权的 `api.openai.com/v1/models` 返回401属于可达性探针，**不是实际Codex生成时延**。样本仍是单日实测，不作全天稳定性保证。

## 候选来源

- FarelRA/proxyip-tracker，tested-ips.csv SHA `8841fc992c783c979a5450cd49219fa9701907ec`。
- FarelRA/proxyip-tracker，domains-ips.csv SHA `69d6eedf32e02878b47a3cdf914dbc15d13ce40d`。
- 现有 US/SG 反代与公开区域域名作为对照；一切来源的实际可用性由本地测试重新确认。
