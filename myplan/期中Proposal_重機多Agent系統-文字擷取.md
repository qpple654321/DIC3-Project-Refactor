# Moto Touring Copilot — 期末 Proposal 內容整理版

> 根據已提供的重機專題提案內容及 [DESIGN.md](../DESIGN.md) 整理，**不是**原始 proposal.pdf 的完整逐字文字擷取。PDF、圖表與參考文獻原版見 [來源專案](https://github.com/yotsubamomo/moto-touring-copilot/tree/main/docs/proposal)。

## 一、Abstract 摘要（A–F）

### A. Attention Getter — Motivation
大型語言模型能生成流暢的旅遊建議，卻不必然符合實際行程限制。對騎士而言，長距離山區行程涉及個人行車配速、民用暮光終、補給間距、午後雷雨與臨時管制，因而「是否能在天黑前返家」是比路線美觀更關鍵的問題。

### B. But — Challenges
一般導航的估時不等於個人騎乘時間。天氣、路況、加油站與日照資料散布於不同平台；單一 LLM 也可能產生數值不一致、忽略營業時間或不遵守硬性限制的候選行程。

### C. Cure — Proposed Solution
提出 **Moto Touring Copilot**，以五個專業 Agent 配合個人配速模型、官方資料接地及確定性驗證，形成「Generate–Verify–Repair」出發前決策輔助架構，而不聲稱預測事故。

### D. Development — Methods
利用歷史 GPX 產生路段特徵並學習移動時間 P50/P90；以保留行程進行整趟 Conformal 校正；預計由 CWA 取得天氣與日照、TDX 取得道路資訊、加油站資料取得位置與營業時間。五條規則依序驗證日照、續航、天氣、路況與數字可追溯。

### E. Experiments — Evaluation
E1：平均速度、速限、Google Routes、MLR 與分位數模型比較。E2：LLM-only／LLM+knowledge／完整 Agent 的獨立 Oracle 消融。E3：歷史風險事件回測（視歷史資料可得性）。E4：真實路線案例分析。指標包含 MAE、MAPE、P90 coverage、限制通過率、數字一致性與拒答率。

### F. Findings — Expected Outcomes
預期能降低行程違規、改善個人化時間預測與提供來源可追溯的解釋。然而本專題尚處 Proposal 階段，**以上均非實測成果**。

## 二、Introduction（緒論）

**A. Attention Getter：** 以實際山區往返、天黑、加油及路況需求引出問題。  
**B. Background：** LLM、多 Agent、GPX 個人化建模、官方開放資料。  
**C. Problem：** 缺乏針對個別騎士、隨抵達時間變化且可外部驗證的完整方案。  
**D. Existing Limitations：** 平均導航、分散資訊、LLM 自我評估不可靠。  
**E. Innovation：** 時間分位數配速、時間窗 API、確定性驗證與有限次重規劃。  
**F. Contributions：** 將可執行性形式化、建立評估基準與可接手的系統設計。

## 三、Related Work（相關研究）

AI Technology：LLM constrained planning、LLM-Modulo/混合驗證、RAG/工具使用、quantile/conformal modeling、multi-agent coordination。

Domain Knowledge：台灣山路時空風險、日照硬限制、騎乘補給、路網地形與燃油續航、個人體能與配速。

| 研究線 | 代表研究 | 核心議題 | 本案的研究缺口定位 |
|---|---|---|---|
| 旅遊規劃可行性 | TravelPlanner、ChinaTravel | 多約束合規 | 加入騎士個人配速與物理約束 |
| 混合規劃 | LLM-Modulo、TRIP-PAL | 外部求解與驗證 | ETA 對齊的領域驗證器 |
| 動態重規劃 | Flex-TravelPlanner、TripTide | 條件干擾 | 預設日照、加油與道路硬限制 |
| 可信任與可追溯 | Itinerary validators | 數字/邏輯檢查 | 以獨立 Oracle 測試 |

文獻題名、實驗數值與各論文限制須以原文核查，不將研究缺口描述為已證明的全球唯一首創。

## 四、Deep Research

五大核心思維、三個專業爭論、十個深度問題，詳見 [重機多 Agent 提案解析](./重機多Agent提案解析-文字擷取.md)。

## 五、八大系統模組

| 編號 | 元件 | 本案 |
|---|---|---|
| ① | User & Scenario | 台灣重機騎士的出發前決策 |
| ② | Input | 自然語言限制與 GPX |
| ③ | Planner & Reasoning | Orchestrator、分工與 replan |
| ④ | Data Acquisition | CWA、TDX、OSM、DTM、加油站 |
| ⑤ | Analytics | P50/P90、Conformal、燃油/日照 |
| ⑥ | Domain Knowledge | 騎乘規則、官方解釋、可引用資料 |
| ⑦ | Decision | 出發時間、加油、風險與拒絕理由 |
| ⑧ | Interaction | Web + Maps + 行程文件 |

## 六、核心演算法

```text
1. UNDERSTAND(query); clarify missing constraints
2. BUILD_ROUTE; segment geometry and terrain
3. CANDIDATE_DEPARTURES
4. PACE_AGENT → P50, trip-calibrated P90, ETA windows
5. WEATHER_AGENT + ROAD_SUPPLY_AGENT → ETA-aligned observations
6. ORCHESTRATOR_COMPOSE candidate
7. VERIFY R1–R5 by deterministic tools
8. IF violated: REPLAN; repeat within attempt limit
9. RETURN verified plan OR explicit infeasibility report
```

## 七、預期交付與限制

Proposal PDF、投影片、設計文件與研究/實驗規劃已存在於來源專案。實際程式、訓練模型、GPS 即時感測及測試報告仍須後續開發與驗證。L2 為規劃範圍；L3 即時 MQTT 與語音警示為延伸。
