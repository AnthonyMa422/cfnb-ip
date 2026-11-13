# CFNB V5 PROXYIP 试验登记（不用于生产订阅）
Date: 2026-11-13 (Asia/Shanghai)
Status: **V5 已启动，电脑离线，执行结果待确认；不得当作通过验收**

## 不变的生产保护
- 正式 EdgeTunnel、Cloudflare KV、GitHub US/SG 订阅列表不在本分支改动。
- Windows 端 v2rayN 活动节点、TUN、订阅不应因这份登记而变化。
- 仅测试实际出口非香港的候选。入口 IP / 国家标签不等于出口所在地。
- 只有完成下载校验、断流与 API 测试后才可讨论晋升。第三方 IP 信誉须另外核实。

## 固定变量
- 入口：141.98.41.87:443
- 服务端模板：US EdgeTunnel（SNI/WS Host 不变）
- 变量：WebSocket 路径中的 proxyip= 反代目标。
- 无路径覆盖（NO_OVERRIDE）作为对照，不等于没有后端默认反代。

## 上轮已实测基准（2026-11-13）
使用同入口、相同模板、交叉顺序各4次100 MiB：
- 原 US PROXYIP: 4/4, 下载 Mbps 中位 71.27, 最低 60.60, 无认证 OpenAI API TTFB 中位 1644.5ms, US 出口。
- 168.138.165.174:443: 4/4, 下载 Mbps 中位 86.72, 最低 83.55, 无认证 API TTFB 中位 2138.2ms, SG 出口。
- 以上为短时网络样本；长期稳定性、第三方信誉均未验收。

## V5 新候选来源
- FarelRA/proxyip-tracker — result/tested-ips.csv，blob SHA 8841fc992c783c979a5450cd49219fa9701907ec；取外部测试排序靠前的 Oracle Cloud IP。
- FarelRA/proxyip-tracker — result/domains-ips.csv，blob SHA 69d6eedf32e02878b47a3cdf914dbc15d13ce40d。
- 区域反代域名参考 wanwushequ/ProxyIP 的公开 README；动态域名结果不能直接视为合格。
- Xiaobei09/proxyip 的 all_good/all_premium/speed.json 是候选来源与信誉线索，**并不等于可用 EdgeTunnel PROXYIP**。
- 同一入口、不同反代须由实际 Xray + HTTP 206 + 字节数复核。

## V5 本地执行位置
- Windows script: C:\Tools\cfnb-quality\node-pool-rebuild-20261109\CFNB_PROXYIP_V5_CURATED_20261113.py
- 2026-11-13 19:xx 已调用一次 Python 启动测试，远端进程返回 PID 16056（启动工具会话 PID），随后 Remote Desktop Commander 无响应。
- **禁止在确认旧进程状态之前重启另一轮同类测速**，避免竞争带宽及误判。
- 本脚本计划44组（31个去重候选 IP、9个动态反代域名、4组基准），第一阶段4 MiB，晋级后第二阶段32 MiB双源。
- 待读取 Windows 目录：cfnb-proxyip-v5-curated-*/RESULTS.json（按时间选最新）。
- 仅以完整的实测 JSON 和当前进程状态为准；本文不代表已经执行完毕。

## 验收顺序
1. 读取 V5 报告，检查报告更新时间、stage_a/stage_b数量、client_unchanged两个字段。
2. 对退出失败、出口未知或 HK 出口严格拒绝晋级；候选必须实测 HTTP206/字节完整。
3. 与上轮原 US、168.138.165.174 使用相同文件大小、测速时段作对照。
4. 优胜组合追加100 MiB跨源重复下载、OpenAI API/Codex真实体验与出口 IP 第三方信誉。
5. 生成独立试验订阅、保留回滚；确认实际使用性能后才触及生产。

本文件没有 UUID、密码、代理路径、访问令牌或其他凭证。
