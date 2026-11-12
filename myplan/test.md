### Moto Touring Copilot: Three Core Innovations

本研究提出 **Moto Touring Copilot：一套結合個人化騎乘配速、多 Agent 動態資訊整合與外部確定性驗證的重機旅遊規劃顧問系統**，旨在克服一般導航無法反映個別騎士特性、旅途中資訊分散，以及 LLM 生成路線缺乏可執行性保證等問題。

本系統具備三大核心創新：

**1. Rider-Specific Pacing & Uncertainty — 個人化配速與時間不確定性**

利用騎士歷史 GPX，按彎道、坡度與道路類型切段，預測移動時間 P50 與 P90；以保留行程的 Conformal Prediction 校正整趟保守抵達時間，而非單純採用一般導航的平均預估。

**2. ETA-Aligned Multi-Agent Context Awareness — 抵達時間對齊的多 Agent 情境感知**

規劃、配速、天氣、路況補給與驗證等五個 Agent 分工，將氣象署、TDX、加油站位置及營業時間整合到「騎士實際抵達各路段的時間窗」，讓天候與補給建議具有時空關聯性。

**3. Deterministic Feasibility Verification — 確定性可執行性驗證**

由 Python 工具對日照、燃油與營業時間、天候、封路及數字出處執行五條客觀規則。LLM 負責生成候選計畫，不自行認證是否可行；違規則重新規劃，無可行解時明確拒絕。

**核心研究價值：** 從「看起來合理的 AI 路線建議」（Plausible Itinerary Generation）邁向「可量化、可追溯、可外部驗證的騎乘行程規劃」（Verifiable Motorcycle Touring Feasibility）。

> **學術誠實說明：** 以上是 Proposal 階段的設計與預期貢獻，非實驗完成結果。
