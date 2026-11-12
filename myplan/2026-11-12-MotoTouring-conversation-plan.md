# Moto Touring Copilot 重機旅遊規劃顧問 — 對話紀錄與提案規劃

- 日期：2026-11-12
- 專案：DIC3 Project Proposal Refactor / Moto Touring Copilot
- 紀錄類型：歷次提供內容的整理式規劃紀錄、設計決策與後續工作
- 狀態：Proposal 階段；非已實現功能或實證結果

## 01. 原始資料：期末 Proposal 與 DESIGN.md

本專案的提案源自「週末想騎山路來回，幾點出發才能在天黑前回家？哪裡可以補油？哪段會下雨或封路？」等真實騎士問題。原始研究文件與 DESIGN.md 設計 13 章，包括 Problem、Requirements、Architecture、Components、Data、AI/Analytics、Domain Knowledge、Algorithm、Implementation Mapping、How to Run、Results、Limitations 和 Change Log。

參考原始專案：[Moto Touring Copilot](https://github.com/yotsubamomo/moto-touring-copilot)。本次 DIC3 提交在根目錄保留 [DESIGN.md](../DESIGN.md)。

## 02. 提案摘要 A–F 決策

1. **A — Attention Getter：** 旅遊行程生成不等同可執行。TravelPlanner 文獻為研究動機，騎士還受到日照、油量、疲勞及道路封閉限制。
2. **B — But：** 一般導航不理解個人配速；資訊平台分散；純 LLM 的數值與多重限制推理不可靠。
3. **C — Cure：** 多 Agent + 個人化 GPX 配速 + ETA 對齊官方資料 + 確定性驗證。
4. **D — Development：** 五 Agent 分工，移動時間 P50/P90、Conformal 校正、五條客觀驗證規則及重新規劃。
5. **E — Experiments：** E1 配速 benchmark；E2 以獨立 Oracle 評估消融；E3 歷史天候／封路回測；E4 代表性路線案例。
6. **F — Findings：** 預期提高準確度、降低違規率與增加數字可追溯性，但尚未測得實驗成果。

## 03. 三大核心亮點：定稿

### 01. 個人化騎乘配速（Rider-Specific Pacing）
從私有 GPX 建立移動時間模型，結合曲率、坡度、道路類型，預測保守返程時間與各點 ETA。先以 MLR 為基準，規劃 LightGBM 分位數及全程 Conformal 校正。

### 02. 多 Agent 情境感知（Multi-Agent Context Awareness）
由 Orchestrator、Pacing、Weather、Road & Supply、Verification 五 Agent 分工；重點在「何時抵達何地」對應的天氣、警報、道路事件與可營業加油站，而非只抓當下資訊。

### 03. 可驗證的決策與重規劃（Verified Planning）
LLM 生成候選，Python 決定規則是否通過；規則檢查日照、補給、天氣、封路、數字追溯；必要時修正出發時間、路線與補油點。

### 最終核心定位
> Moto Touring Copilot integrates **personalized travel-time prediction, arrival-time-aligned multi-agent data acquisition, and deterministic feasibility verification** to provide traceable, risk-aware pre-departure motorcycle touring decisions.

## 04. 深度研究與評估規劃

- **5 Cores：** 最難路段優先、山區天氣看時間窗、日照是硬限制、加油站要抵達時有開、道路狀態動態變動。
- **3 專業爭論：** 平均速度 vs 個人化 P90；LLM 自我檢查 vs 外部驗證；即時資料完整性 vs 服務不中斷。
- **10 深度問題：** 關於缺少 GPX、時間分位數、資料延遲、路段封閉、無補給點、隱私、風險不確定性、獨立 Oracle、方案無解、跨域泛化等，詳見[提案解析](./重機多Agent提案解析-文字擷取.md)。
- **比較：** LLM-only、LLM + RAG、完整多 Agent + Verification；配速則以平均速度、速限換算、Google Routes 兩輪車估計及 MLR 為 baseline。
- **H1–H3：** 混合架構改善硬限制通過率、資料接地降低違規、重規劃提高干擾下的成功修正率。
- **學術誠實：** 所有百分比、提升幅度均需實驗支持；未實作的功能不得視為成果。

## 05. 對話／作業決策紀錄

| 順序 | 需求 | 最終決定 |
|---|---|---|
| 1 | 依期末提案規範提出重機旅遊 AI Agent | Moto Touring Copilot，多 Agent 出發前決策輔助 |
| 2 | 研究內容需有 A–F、研究缺口與評估 | 採研究動機→缺口→解方→方法→實驗→預期貢獻 |
| 3 | 應有三個最值得報告的亮點 | 個人配速、多 Agent 時空情境、確定性驗證 |
| 4 | 加強學術價值及與文獻差異 | 可執行性問題定式、外部 Oracle、H1–H3 |
| 5 | 將 DESIGN.md 轉成 OpenSpec 的 spec-driven workflow | 分離 Proposal、Design、Specs、Tasks，標示未驗證 |
| 6 | DIC3 作業 GitHub 應照老師樣本排版 | README + myplan 附件與對話紀錄 + 根目錄 DESIGN.md |

## 06. 下一步（尚未完成）

1. 在個人電腦實際安裝 OpenSpec CLI，留下 `openspec --version` 與 `validate` 證據。
2. 對照官方原始文獻核實關鍵通過率、論文題名與 DOI。
3. 更新 Introduction 與 Related Work 到正式 Proposal 時，維持引用與研究階段一致。
4. 開始實作後增補單元測試、資料來源時間戳、E1–E4 獨立實驗。
5. 保護 GPX 行程與 API 金鑰，不公開住家或工作地點。

---
本檔為對話與決策的**整理式紀錄**，不是逐字聊天紀錄或尚未提供的原始二進位附件。
