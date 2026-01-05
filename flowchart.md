```mermaid
graph TD
    %% 1. Batch Start
    Start((開始 Batch)) --> BSS[1. Batch start Sequence]
    BSS --> BSS_Act[開啟: AV3417K, AV3416K, AV3415K, AV3402K, AV3411K<br/>關閉: AV3217K, AV3218K]
    
    %% 2. Ready Sequence
    BSS_Act --> RS[2. Ready Sequence]
    RS --> RS_Close[關閉所有閥門]
    RS_Close --> RS_Check{Check List 勾選}
    RS_Check -->|所有項目合格| RS_Ready[按下 READY 按鈕]
    RS_Ready --> AV3217_O[AV3217K 開啟]

    %% 5. 入料 (照程序流程順序)
    AV3217_O --> In_Seq[5. 入料 Sequence]
    In_Seq --> AV3417_O[AV3417K 開啟]
    AV3417_O --> In_Wait{達到設定重量?}
    In_Wait -- 否 --> In_Wait
    In_Wait -- 是 --> AV3417_C[AV3417K 關閉]
    AV3417_C --> In_Delay[等待 10 秒]

    %% 3. Solvent Stripping
    In_Delay --> SS[3. Solvent stripping Sequence]
    SS --> AV3415_O[AV3415K 開啟]
    AV3415_O --> SS_Delay[等待 10 秒]
    SS_Delay --> AV3411_O[AV3411K 開啟]
    AV3411_O --> SS_Weight{SC3401K < 下表重量?}

    %% 4. Cold Trap Purge
    SS_Weight -- 是 --> CTP[4. Cold trap purge Sequence]
    SS_Weight -- 否 --> End_Check

    CTP --> CTP_Close[關閉: AV3415L, AV3411K]
    CTP_Close --> AV3402_O[AV3402K 開啟]
    AV3402_O --> CTP_W1[等待 15 秒]
    CTP_W1 --> AV3402_C[AV3402K 關閉]
    AV3402_C --> AV3416_O[AV3416K 開啟]
    AV3416_O --> CTP_W2[等待 20 秒]
    CTP_W2 --> AV3416_C[AV3416K 關閉]
    AV3416_C --> End_Check

    %% 6. End Sequence
    End_Check{累積量達 18KG?}
    End_Check -- 否 --> SS
    End_Check -- 是 --> ES[6. End Sequence]
    
    ES --> ES_Purge[執行 Cold trap purge]
    ES_Purge --> ES_Close[關閉: AV3415K, AV3411K, AV3217K]
    ES_Close --> ES_Open[開啟: AV3417K, AV3218K]
    ES_Open --> ES_Wait[等待 5 秒]
    ES_Wait --> ES_Final[關閉: AV3417K, AV3218K]
    ES_Final --> Finish((結束))

    %% 樣式設定
    style Start fill:#f96
    style Finish fill:#f96
    style RS_Check fill:#fff4dd
    style SS_Weight fill:#fff4dd
    style End_Check fill:#fff4dd
```
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
  mermaid.initialize({ startOnLoad: true });
</script>
