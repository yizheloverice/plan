# 国庆普吉-皮皮岛-曼谷规划:三开源工具对比

> 生成日期:2026-09-01 | 场景:9/26 香港出发落曼谷,10/4 曼谷回港,主玩普吉,皮皮岛约 3 天,曼谷 ≤1 天(全部可变动)
> 工具:A=trip-planner-skill(skywain)· B=skills-travel-planner(huanyuzhilv)· C=TripStar(1sdv)

## 一、三份规划的总览

| 维度 | A · trip-planner-skill | B · skills-travel-planner | C · TripStar |
|---|---|---|---|
| 形态 | Agent Skill 流水线(Phase 0-6) | Agent Skill(路书流水线) | 完整 Web 应用(FastAPI+Vue,多智能体) |
| 交付物 | `trip-illustrated.html`(1.3MB,插画主题页)+ `plan.geo.json` + `trip.kml` | `phuket-phiphi-bkk-9days.html` 路书(可编辑 tripData.json) | 交互网页(概览/预算/地图/每日/知识图谱)+ AI 问答 |
| 规划路径 | 我按 SKILL 全流程执行(航班扫描→骨架→每日→酒店→自检) | 我按 SKILL 执行(intake 简表→tripData→交付流水线) | 我部署应用并驱动其多智能体生成(plan 88fe70f8) |
| 数据源 | Google Flights(无 key)+ nager/Open-Meteo(Nominatim)+ 官方站点+ XHS MCP | 飞猪实时(机票/酒店/POI)+ XHS MCP(攻略+配图)+ 维基/占位兜底 |  小红书原生搜索提纯 + Google(修复 Billing 后)+ 高德兜底 + LLM 编排|
| 输出语言 | 中文页面(plan.lang=zh) | 中文路书 | 英文(默认 UI;LLM 输出英文) |

## 二、行程骨架对比

| | A | B | C |
|---|---|---|---|
| 9/26 | HKG 13:30→15:30 + BKK 17:40→19:00 转机 → 芭东 | HKG 14:20→16:05 + BKK 18:00→19:25 → 芭东 | 无航班细节,直达概念 |
| 9/27 | 大佛+查龙寺+老城市集+卡伦日落 | 同左(与 A 一致) | 查龙寺+老城+步行街 |
| 9/28 | 快艇 09:00→皮皮岛 | 同左 | 皮皮岛一日游(当日往返) |
| 9/29 | 皮皮岛环岛(翡翠湖/燕窝洞/猴滩) | 同左 | 皮皮岛+竹子岛+猴滩 一日 |
| 9/30 | 竹子岛浮潜+SPA | 同左 | Racha 岛+卡伦海滩 |
| 10/1 | 快艇回普吉,卡塔入住,日落 | 同左 | 射击场+丛林飞梭(Hanuman) |
| 10/2 | 攀牙湾日游(007 岛+洪岛) | 同左 | 大象营+Khai 岛 |
| 10/3 | 10:15 飞曼谷;卧佛寺+郑王庙日落+考山路 | 09:50 飞曼谷;**大皇宫+卧佛寺+郑王庙**+考山路 | 曼谷:ICONSIAM+Asiatique |
| 10/4 | 10:10 回港(T 公式 6:15 出发) | 10:10 回港 | **安排了大皇宫+卧佛寺+郑王庙(与回程冲突)** |

**关键差异**:
- **曼谷分配**:A/B 严格按"曼谷 ≤1 天"(10/3 下午+晚上,10/4 上午回);C 给曼谷 2 天(last day 02 故宫同游),因为提交时未注入"10/4 需离开"的硬约束 —— C 的缺陷是行程与回程航班脱节。
- **皮皮岛住宿**:A/B 正确设为 3 晚(City 生活区);C 全部安排普吉芭东 7 晚(皮皮岛只是日游,且未识别用户"皮皮岛待 3 天"需求)。
- **玛雅湾关闭(8/1-9/30)**:A/B 均识别并绕行(环岛改 Pileh/Viking/Monkey);C 的 LLM 在最终建议中**明确提示**"Maya Bay closed until 30 Sep"(约束注入生效),但日程未体现具体绕行。
- **航班衔接**:A/B 有具体班次与缓冲(HKG 13:30+2h 转;B 14:20+1h55m 转);C 无航班信息(完全依赖 LLM,缺真实比价)。

## 三、价格对比(2 人,人民币)

| | A | B | C |
|---|---|---|---|
| 机票(4 段) | 5682(Google 缓存价) | 3024(**飞猪实查价**,如 BKK-HKG ¥457 vs Google 缓存 ¥1052) | 无明细(总预算 60000 THB≈12210 CNY) |
| 皮皮岛船票 | 1302 | 1302 | 未单列(含日游) |
| 住宿 | ~4000(评级+带日 Google Hotels 深链) | ~4040(**飞猪 9/26-10/4 实查价**) | 7100 THB/晚×? 未单列(Holiday Inn 2500-3500 THB/晚) |
| 门票/活动 | ~1140 | ~570(大皇宫/卧佛/郑王庙) | 18600 THB(含射击/大象营/丛林飞梭) |
| 合计 | ~15800 | ~15800 | ~12210(口径不同,含活动) |

**数据可信度**:B 的机票酒店完全来自飞猪实时接口(标注"实查");A 用 Google Flights 缓存(标注"比较级,深链为准");C 的预算由 LLM 估算(60000 THB 无来源标注)。

## 四、部署/使用成本

| | A | B | C |
|---|---|---|---|
| 依赖 | Python 3.9+;fast-flights(需小修兼容版本) | Python;httpx;flyai CLI;**依赖 TikHub(收费,本项目已禁用)** | Python + Node + **LLM key + XHS Cookie + 高德/Google key**;前后端双服务 |
| 本次实际 | 零付费,30 分钟产出 | 零付费(用 XHS MCP 补图) | 免费 LLM(DeepSeek)+ 本地部署 ~40 分钟 |
| 可复用性 | 单人即可维护 plan.geo.json | 天然为"旅行社交付"设计 | 面向最终用户的自助规划 UI |

## 五、结论与推荐

1. **推荐 A 作为最终执行版**:逐时计划+真实航班扫描+玛雅湾等约束校验+自检(QC 全绿)+离线 KML+可打印/主题页+预算/支票清单,且完全免费、无 key 依赖。
2. **推荐 B 用于"给客户/家人看的路书"**:飞猪实价 + 小红书真实图 + 交付级排版;局限是无 TikHub 时配图走降级(需核对)。
3. **C 的定位是"自助规划产品"而非规划工具**:UI/知识图谱/AI 问答出色,但行程与用户硬约束(回程日、皮皮岛住 3 晚、曼谷 1 天)需要人工再核对;英文界面不友好。
4. **综合建议**:用 A 的 plan.geo.json 作为单一真源,出行前 2 周用 B 的飞猪价格再核对机票酒店(实查)并预订;使用 C 可在现场改期(其 AI 问答适合在旅途中调整)。

产出位置:
- A:`travel/planner-a/thailand-2026/`(trip-illustrated.html · plan.geo.json · trip.kml)
- B:`travel/planner-b/phuket-2026-09/`(phuket-phiphi-bkk-9days.html · tripData.json · sources/)
- C:`travel/planner-c/tripstar-88fe70f8/`(plan.json · summary.md)
- C(修复后):`travel/planner-c/tripstar-76b675ab/`(plan.json · summary.md)

## 六、更新(2026-09-01 · Google key 修复 Billing 后)

1. **Google Geocoding/Places 已验证可用**;工具A 的皮皮岛 4 个 POI(Viewpoint 1/3、Viking Cave、Pileh Lagoon)已替换为 Google 精确坐标,并重跑流水线(links 16 · check exit 0 · KML 42 pins · QC PASS)。
2. **TripStar v2(76b675ab)重跑完成**,约束全部命中:皮皮岛 3 晚(Phi Phi Don Chakama 度假村)✓、10/4 上午无景点(回程锁定)✓、曼谷一天(大皇宫/卧佛寺/郑王庙)✓、玛雅湾绕行 ✓;预算 47,200 THB。仍存小缺陷:`language: zh` 参数未生效,输出仍为英文。
3. 并存证:高德 web 服务 key 当日配额超限(CUQPS_HAS_EXCEEDED_THE_LIMIT),当时 TripStar 的降级链也断;现 Google 可用后不影响 TripStar,若后续以高德为主请升认证/查配额。
4. **小红书渠道并入方案A(2026-09-01)**:按"小红书为最高质量渠道"要求,经本地 XHS MCP 采集 3 批搜索 + 2 篇详情(F 菲《皮皮岛住宿无脑长滩》458 收藏、皮大饼《普吉岛个人感受》3319 收藏),情报以「线索层」写入 `plan.geo.json` —— decisions +2 条、酒店候选 +长滩 The Beach Resort、D2/D3/D5/D6/D7/D8 对应 `note` 标注 XHS 来源(收藏数),`unverified` 首行注明"用户经验未官网核验"。重跑 check(exit 0)+ render + QC PASS(1338KB)。渠道规则:A 保持官方验证层,小红书只作线索层,两者并存互斥缺一不可。
