# 重機旅遊規劃顧問 — 提案解析（DIC3 學生專題版）

> 參照教師 DietSense 專案「提案解析—文字擷取」的論述層級重新撰寫；本文件是 **Moto Touring Copilot 的研究整理**，並非老師範例或原 Proposal PDF 的逐字轉錄。原始依據為 [DESIGN.md](../DESIGN.md) 與 [Proposal](https://github.com/yotsubamomo/moto-touring-copilot/blob/main/docs/proposal/proposal.pdf)。以下全部屬 Proposal 階段，引用數字須逐篇核實。

## 一、A–F 摘要架構強化

| A–F | 研究重點 | 本專題對應 |
|---|---|---|
| A Attention Getter | 研究動機／騎士痛點 | 長距離山路「能否天黑前回家」牽涉速度、日照、油量、天氣與路況 |
| B But | 現行不足／Research Gap | 平均速度導航不個人化、外部資料分散、純 LLM 無法可靠滿足硬限制 |
| C Cure | 核心解法 | 五 Agent + GPX P50/P90 + 官方資料接地 + Deterministic Verifier |
| D Development | 技術與架構 | 分段路網、DTM／曲率特徵、Conformal、R1–R5、Generate–Verify–Repair |
| E Experiments | 比較與驗證 | E1 分組配速 benchmark；E2 independent Oracle 消融；E3 回測；E4 案例 |
| F Findings | 預期貢獻 | 提升個人時間估計與可追溯性，降低硬限制違規；未實證 |

**一句話研究主張：** 本專題不是只做旅遊 App，而是把「LLM 生成的機車行程是否真的騎得完？」轉成**可測量、可重複驗證**的約束規劃研究問題。

## 二、Deep Research：三大核心問題

### Q1. 五個核心思維模式（Five Domain Thinking Patterns）

**1. Hardest-Segment Planning — 最困難路段決定行程邊界**

- 意義：連續彎道、坡道、不同路況及個人熟練度造成路段移動時間不均，不能全程用平均車速估算。
- 來源／設計證據：DESIGN §5–§7 以歷史 GPX、DTM、曲率與路段時間模型形成個別騎士預測。
- 系統影響：依 0.5–2 km 等道路特徵分段，預測移動時間，並用 P90 進行保守規劃。
- 待驗證：個人化模型是否顯著降低 MAE/MAPE，是否能在新路線泛化。

**2. Weather as an Arrival-Time Window — 山區天氣看抵達時間窗**

- 意義：詢問時的天氣不等於抵達山區時的天氣，風險需與 ETA 時空重疊判斷。
- 對應資料：氣象署預報與警特報，結合各點 P50/P90 ETA。
- 系統影響：Weather Agent 查詢分段位置與預計時間，回報警戒路段及來源時間。
- 待驗證：預報解析度與時效是否足以支持路段級判斷。

**3. Daylight as an Operational Hard Constraint — 日照與騎乘時數是硬限制**

- 意義：天黑前返程必須考慮保守整趟時間與民用暮光終，而非只比對平均 ETA。
- 規則：departure + calibrated trip P90 ≤ min(civil twilight end, user deadline)。
- 系統影響：Verification Agent 不通過則修改出發時間或路線；無解時回報無可行方案。
- 待驗證：P90 calibration 的實際覆蓋率與錯誤拒絕率。

**4. Refueling at Arrival, Not Just Distance — 補給須看抵達時是否營業**

- 意義：即使路線上有加油站，也可能超過保留油量可達範圍，或抵達時已關門。
- 規則：站間距離不得超過 range × (1 – safety margin)，P90 抵達須落在營業時段。
- 系統影響：Road & Supply Agent 連結沿線 POI、車系續航與營業時間。
- 待驗證：資料品質、休息與停留時間如何影響實際 ETA。

**5. Dynamic Road-State Thinking — 山區道路狀態持續變動**

- 意義：省道封閉、交管、暴雨警戒會使原計畫突然失效。
- 對應資料：TDX 即時事件及公路局公告；規劃階段查詢而非聲稱已能騎乘中即時監控。
- 系統影響：有封閉即拒絕候選，將違規原因回饋到 bounded replan。
- 待驗證：資料延遲與缺乏歷史封路機器資料的問題。

### Q2. 三個最重要的專業爭論（Three Domain Debates / Trade-offs）

#### 爭論一：Average Speed vs. Personalized Conservative ETA

| | 觀點 A：平均／導航 ETA | 觀點 B：個人 P50＋全程校正 P90 |
|---|---|---|
| 主張 | 成本低、容易取得，作為通用 benchmark | 依騎士與地形調整，保守評估返程 |
| 優勢 | 無需蒐集個人 GPS 資料 | 能表達個體差異與時間不確定性 |
| 代價 | 山路與騎乘習慣差異被平均化 | 有資料需求、冷啟動與過度保守風險 |
| 未解問題 | 導航 ETA 在不同山區場景的校準性 | n=1 的泛化、P90 覆蓋率與時間成本 |
| 系統立場 | 保留平均速度、速限與 Google Routes 作 Baseline | 以獨立 GPX 行程評估，無足夠資料時明示限制 |

#### 爭論二：LLM Self-Critique vs. Deterministic External Verification

| | 觀點 A：LLM 自行檢查 | 觀點 B：符號化 Python 驗證 |
|---|---|---|
| 主張 | 對話一致、部署簡單 | 日照、燃油、路況、數值條件可重算 |
| 優勢 | 易整合自然語言理由 | 規則透明、可測試、可追溯 |
| 代價 | 可能幻覺或錯誤自我認證 | 需要明確規則、API 與例外處理 |
| 未解問題 | 錯誤自信程度 | 外部資料錯誤會不會造成錯誤通過 |
| 系統立場 | 僅負責需求理解與候選生成 | R1–R5 為最終驗證依據，由獨立 Oracle 評估效果 |

#### 爭論三：Rich Real-Time Data vs. Robust Degraded Operation

| | 觀點 A：依賴完整即時數據 | 觀點 B：缺資料時明確降級／拒答 |
|---|---|---|
| 主張 | 最完整的路況、天候與營業時段判斷 | 優先防止用推測取代客觀數據 |
| 優勢 | 條件時效性佳 | 避免 API 失效仍誤判可行 |
| 代價 | API 延遲、額度、維運負擔 | 可能增加拒答率、降低交付率 |
| 未解問題 | 新鮮度門檻及各種資料的可靠性 | 哪些非關鍵資料可明示不確定性而繼續 |
| 系統立場 | 優先官方可溯源資料與時間戳 | 重要狀態未知時不宣稱已驗證 |

**目前共識與研究問題：** 較複雜的架構是否在**可行率、正確拒答、預測誤差、資料取得成本**間取得優勢，必須經實驗而非直覺判定。

### Q3. 十個深度理解問題（Ten Research Questions）

1. **[Why]** 為什麼同一段山路的平均速度不能代表特定騎士返程時間？如何實證指出誤差集中在哪些地形？
2. **[What-if]** 若新騎士沒有 GPX 或只有兩趟歷史紀錄，如何在不虛構 P90 校準精度的情況下提供方案？
3. **[Trade-off]** 增加 P90 安全餘裕能降低日照違規，但也會讓更多行程被拒絕；如何設計覆蓋率與交付率共同評估？
4. **[Failure Case]** 若加油站營業時間缺漏或 TDX API 逾時，驗證器應判不可行、未知，還是採替代來源？如何避免假陽性？
5. **[Evidence]** 如何以 E1 GroupKFold 與時間外推證明個人配速模型優於平均速度和導航預估，而不發生同趟資料洩漏？
6. **[Comparison]** 比較純 LLM、LLM + RAG、LLM + 確定性驗證器在硬限制通過率、拒答率、工具成本與延遲方面的優劣。
7. **[Design Decision]** 為什麼需要多 Agent 專門化分工，而非單一 Agent 呼叫全部工具？在什麼條件下單一 Agent 可能更適合？
8. **[Real-world Scenario]** 騎乘南橫時，若路段 ETA 突然落入豪雨警報且補給站即將關門，系統如何修正出發時間／路線／加油點？
9. **[Mathematics]** 為什麼不應將速度 P90 直接換成時間 P90？為什麼加總各段 P90 不保證整趟 P90 校正？Conformal 的分組假設為何？
10. **[Critical Reflection]** 如何建立獨立 Oracle，避免讓執行計畫的 Agent 或驗證器同時擔任裁判？對於真正的騎乘滿意度，離線代理指標能支持到什麼程度？

## 三、與既有文獻的差異化

| 研究類型 | 代表文獻線索 | 文獻常見關注 | 本專題預計驗證的補充 |
|---|---|---|---|
| 旅遊計畫基準 | TravelPlanner、ChinaTravel | 多重規則通過率 | 機車物理限制與個人化 ETA |
| 混合規劃 | LLM-Modulo、TRIP-PAL | 生成與外部求解分離 | 時空對齊及安全相關硬限制 |
| 動態行程 | Flex-TravelPlanner、TripTide | 干擾下的行程修正 | 油量、日照與實際道路事件 |
| 地圖與資訊接地 | MapEval、Iti-Validator 等 | 空間推理、驗證與可溯源 | 數字完全由工具輸出支持 |

> 文獻為研究對照線索。不能由本表直接推論「前人完全沒做過」；正式 Related Work 應檢查每篇原始論文的方法與實驗設定。

## 四、系統架構與 OpenSpec 映射

| 教師八大模組 | 本系統 | OpenSpec 對應 |
|---|---|---|
| ① 使用者與情境 | 台灣重機騎士／日期／約束 | planning-workflow |
| ② 問題輸入 | 自然語言與 GPX | planning-workflow, pacing-and-privacy |
| ③ Planner | Orchestrator + Repair Loop | planning-workflow |
| ④ 資料取得 | CWA、TDX、Fuel POI、OSM | environment-grounding |
| ⑤ Analytics | Moving-time P50/P90、校正、Fuel/Daylight | pacing-and-privacy |
| ⑥ Domain Knowledge | 騎乘與風險知識庫/RAG | environment-grounding |
| ⑦ 決策 | 五條硬驗證規則 + 無解報告 | deterministic-verification |
| ⑧ 輸出 | 對話、地圖、引用 | planning-workflow |

**系統流程：** Understand → Build Route → Segment Features → Candidate Departures → Pace Agent → Weather/Road & Supply Agents → Compose → Verify R1–R5 → Replan or Render。

## 五、實驗計畫與可否證假設

| 實驗 | 問題 | Baselines | 指標 |
|---|---|---|---|
| E1 配速 | 個人時間模型是否較準 | 平均速度、速限、Google Routes、MLR | MAE、MAPE、Trip P90 覆蓋 |
| E2 消融 | 工具／驗證哪個提升可行性 | LLM-only、LLM+KB、完整多 Agent、移除配速／天氣／路況 | Independent Oracle 違規率、數字一致率、拒答率 |
| E3 風險規則 | 能否捕捉歷史天候封路事件 | 透明風險規則 | 命中率、誤報率；視歷史來源可得性 |
| E4 案例 | 是否合理預測真實騎乘 | 2–3 趟歷史路線 | 誤差、行程解釋與可執行性 |

H1：混合驗證比 LLM-only 更能滿足硬限制。  
H2：來源接地較無接地規劃減少違規。  
H3：有 replan 較固定方案更能應對干擾。

## 六、A2Z 精進／交接摘要（仿老師的研究解析形式）

A—Attention：天黑與補油問題；B—Background：通用 LLM 旅遊規劃；C—Cure：多 Agent + Tools + Rules；D—Data：私有 GPX、CWA、TDX；E—Experiments：E1–E4；F—Findings：僅列預期；G—Graph：系統架構；H—How：驗證—修正循環；I—Implementation：Python Analytics；J—Judgement：取捨與失敗案例；K—Key points：配速、情境、驗證；L—Limitations：n=1／API 歷史；M—Methods：Quantile + Conformal；N—Novel integration：ETA-aligned safety constraints；O—Oracle：獨立裁判；P—Privacy：原 GPX 不公開；Q—Quality：數據可追溯；R—Rules：五個 hard checks；S—Sources：官方開放資料；T—Temporal：抵達時間窗；U—Use case：山區當日往返；V—Validate：H1–H3；W—Workflow：OpenSpec；X—eXplain：違規理由；Y—Yields：可審閱規格；Z—Zen：不宣稱事故預測。

## 七、結語與仍需精進

系統研究價值在於把多重物理約束納入可檢驗的行程規劃，而不是單純生成更多路線。**所有技術、模型與評估目前屬規劃；尚未完成的部分不能視為實證。** 下一階段須核對文獻、OpenSpec CLI 驗證、實作／測試與獨立 Oracle 評估。
