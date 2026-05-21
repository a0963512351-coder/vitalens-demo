<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VitaLens AI - 智慧醫療期末營運實踐系統 (第六組)</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
    
    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "MedicalWebPage",
      "name": "VitaLens AI 智慧醫療導航系統",
      "description": "明新科技大學資工系第六組期末成果，實作RAG臨床級解讀與Edge-First隱私防護。",
      "provider": {
        "@type": "CivicStructure",
        "name": "明新科技大學資工系第六組"
      }
    }
    </script>

    <style>
        /* 1.2 視覺規範 (VI) 嚴格執行 */
        :root {
            --medical-blue: #1A2B48;  /* 醫學藍 */
            --oxygen-green: #48C9B0;  /* 氧氣綠 */
            --bg-light: #F4F6F9;
            --font-main: 'Noto Sans TC', 'Microsoft JhengHei', sans-serif;
        }

        /* 配合報告指引排版要求：內文字級 14pt */
        body {
            font-family: var(--font-main);
            background-color: var(--bg-light);
            margin: 0;
            padding: 20px;
            font-size: 18.66px; /* 14pt = 18.66px */
            color: #333;
            line-height: 1.6;
        }

        .header-area {
            text-align: center;
            margin-bottom: 20px;
        }

        h1, h2, h3 { 
            color: var(--medical-blue); 
            margin-top: 0;
        }

        .team-badge {
            background-color: var(--medical-blue);
            color: white;
            padding: 6px 18px;
            border-radius: 20px;
            font-size: 16px;
            font-weight: bold;
            display: inline-block;
        }

        /* 4.2 公關危機應變處置看板 (預設隱藏，由控制台觸發) */
        .crisis-banner {
            display: none;
            background-color: #F8D7DA;
            color: #721C24;
            border: 2px solid #F5C6CB;
            padding: 15px;
            border-radius: 8px;
            width: 90%;
            max-width: 1100px;
            margin: 0 auto 20px auto;
            font-size: 16px;
        }

        .dashboard {
            display: grid;
            grid-template-columns: 1fr 380px;
            gap: 20px;
            width: 95%;
            max-width: 1150px;
            margin: 0 auto;
        }

        .main-panel {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .card {
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            padding: 20px;
        }

        /* 3.1 Nudge 效果專區樣式 */
        .nudge-banner {
            background: linear-gradient(135deg, #e0f7f4, #ffffff);
            border-left: 5px solid var(--oxygen-green);
            padding: 15px;
            border-radius: 6px;
            margin-bottom: 15px;
            display: none;
        }

        /* 2.1 AI 衛教客服對話區 */
        .chat-messages {
            height: 250px;
            overflow-y: auto;
            border: 1px solid #E1E8ED;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            background: #FAFAFA;
            font-size: 16px;
        }

        .message { margin-bottom: 12px; }
        .user-text { color: var(--medical-blue); font-weight: bold; }
        .source-tag {
            display: block;
            font-size: 13px;
            color: var(--oxygen-green);
            margin-top: 3px;
            font-weight: bold;
        }

        .input-group { display: flex; gap: 10px; }
        input[type="text"] {
            flex: 1;
            padding: 12px;
            border: 1px solid #CCD6DD;
            border-radius: 6px;
            font-size: 16px;
        }

        button {
            background-color: var(--medical-blue);
            color: white;
            border: none;
            padding: 10px 22px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            font-size: 16px;
            transition: 0.2s;
        }
        button:hover { background-color: #2c4366; }

        /* 側邊欄日誌與後台管理 */
        .side-panel {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .log-box {
            background: #1E222B;
            color: #A3B3C2;
            border-radius: 12px;
            padding: 20px;
            font-family: 'Courier New', Courier, monospace;
            font-size: 13px;
            height: 220px;
            display: flex;
            flex-direction: column;
        }

        .log-list {
            flex: 1;
            overflow-y: auto;
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .log-item {
            margin-bottom: 8px;
            border-left: 2px solid var(--oxygen-green);
            padding-left: 6px;
        }

        /* 營運模擬器按鈕 */
        .admin-btn {
            background-color: #6C757D;
            font-size: 14px;
            padding: 6px 12px;
            margin-top: 5px;
        }
    </style>
</head>
<body>

    <div class="header-area">
        <h1>✨ VitaLens AI 智慧醫療營運展示系統</h1>
        <div class="team-badge">明新科技大學 資工系 · 第六組期末成果</div>
    </div>

    <div class="crisis-banner" id="crisisBanner">
        <strong>⚠️ 數據倫理誠信聲明：</strong> 本公司偵測到外部去識別化生理特徵之資安異常。第六組團隊已於 2 小時內完成技術阻斷，全面更新 TLS 1.3 密鑰，並啟用零知識證明驗證。為落實數位誠信，全體用戶即刻免費升級 6 個月專業版服務。
    </div>

    <div class="dashboard">
        
        <div class="main-panel">
            
            <div class="nudge-banner" id="nudgeSection">
                <h3 style="color: #117A65; margin: 0 0 5px 0;">🧘 個人化減壓推力提示</h3>
                <p style="margin: 0; font-size: 16px;">
                    系統依據您連續 3 日之 HRV 生理特徵自動更換首頁。偵測到您近期交感神經處於高壓狀態，已為您解鎖<b>「舒壓減壓專區」</b>，請跟隨引導進行 3 分鐘深呼吸。
                </p>
            </div>

            <div class="card">
                <h2>👤 會員中心與營運狀態</h2>
                <div style="font-size: 16px; display: flex; justify-content: space-between; background: #F8F9FA; padding: 15px; border-radius: 8px;">
                    <div>帳號權限：<span id="accountTier" style="font-weight:bold; color:var(--medical-blue);">免費體驗版</span></div>
                    <div>健康點數 (VitaPoints)：<span id="vitaPoints" style="font-weight:bold; color:var(--oxygen-green);">120</span> pt</div>
                    <div>流失風險評估 (CLV)：<span id="clvStatus" style="font-weight:bold; color:#E67E22;">未評估</span></div>
                </div>
            </div>

            <div class="card">
                <h2>🤖 RAG 臨床級衛教諮詢室</h2>
                <div class="chat-messages" id="chatMessages">
                    <div class="message">
                        <span class="ai-text">【VitaLens AI】您好！請輸入生理症狀（如：<b>胸悶、心跳快、睡不著</b>），本系統將透過 RAG 架構檢索 2026 最新文獻為您提供無幻覺的白話轉譯。</span>
                    </div>
                </div>
                <div class="input-group">
                    <input type="text" id="userInput" placeholder="請輸入症狀描述...">
                    <button onclick="handleAskAI()">詢問 AI</button>
                </div>
            </div>

        </div>

        <div class="side-panel">
            
            <div class="log-box">
                <h3 style="color: var(--oxygen-green); font-size: 16px; margin-bottom: 10px;">🛡️ 數據隱私安全日誌 (Log)</h3>
                <ul class="log-list" id="logList">
                    <li class="log-item">[系統初始化] TLS 1.3 協定運行中。</li>
                    <li class="log-item">[安全防護] 本地數據 AES-256 加密完成。</li>
                </ul>
            </div>

            <div class="card" style="padding: 15px;">
                <h3 style="font-size: 16px; margin-bottom: 10px;">🛠️ 第六組營運情境模擬器</h3>
                <div style="display: flex; flex-direction: column; gap: 8px;">
                    <button class="admin-btn" onclick="simulateHighPressure()">1. 模擬高壓第一方數據 (HRV變低)</button>
                    <button class="admin-btn" onclick="simulateClvRetention()">2. 執行 CRM 貝氏 CLV 模型預測</button>
                    <button class="admin-btn" onclick="triggerFreemiumUpgrade()">3. 模擬專業版訂閱 (NT$299/月)</button>
                    <button class="admin-btn" onclick="triggerCrisis()">4. 觸發 4.2 公關危機防護演練</button>
                </div>
            </div>

        </div>

    </div>

    <script>
        // 2.1 RAG 醫學知識庫 (模擬向量資料庫檢索)
        const medicalDB = {
            "heart": {
                keywords: ["胸悶", "胸痛", "心悸", "心跳快", "喘不過氣"],
                answer: "您的即時生理特徵顯示心率有局部波動。胸部不適可能與工作高壓或竹北通勤疲勞相關。請嘗試進行 3 分鐘腹式呼吸。若症狀持續或伴隨左手外側放射性疼痛，請立即前往智慧診所尋求實體協助。",
                source: "美國心臟協會 (AHA) 2026 臨床照護指引"
            },
            "sleep": {
                keywords: ["失眠", "睡不著", "好累", "疲勞", "頭痛"],
                answer: "分析您近期活動日誌，深度睡眠時間佔比偏低。建議睡前 1 小時強制關閉數位裝置（避免資工代碼編寫導致大腦過度興奮），並維持環境室溫 24°C 以平穩自主神經系統。",
                source: "台灣睡眠醫學學會 2025 臨床治療路徑"
            },
            "default": {
                answer: "系統成功提取語義特徵向量。初步評估為亞健康狀態下的一時性生理信號波動。建議保持水分補充、規律作息，並可隨時導出連續性報告提供給家庭醫師參考。",
                source: "VitaLens 預防醫學知識庫 (2026 第六組修訂版)"
            }
        };

        let askCount = 0;
        let isPremium = false;

        // 2.1 RAG 客服邏輯實作
        function handleAskAI() {
            const inputEl = document.getElementById('userInput');
            const query = inputEl.value.trim();
            if (!query) return;

            // 4.1 Freemium 限制邏輯 (免費版每日限 3 次)
            if (!isPremium && askCount >= 3) {
                alert("您已達到免費版每日 3 次諮詢上限！請點擊情境模擬器中的『專業版訂閱』解除限制。");
                return;
            }

            appendChat(`【用戶】：${query}`, 'user-text');
            inputEl.value = '';
            askCount++;

            // 觸發第一方隱私數據日誌
            addLog(`提取「${query.substring(0,6)}」去識別化特徵向量`);
            addLog(`透過 TLS 1.3 加密通道安全上傳至 RAG 核心`);

            // 模擬 RAG 檢索時間
            appendChat("【VitaLens AI】：智者架構檢索中...", 'ai-text', 'loading');

            setTimeout(() => {
                const loadingItem = document.getElementById('loading');
                if (loadingItem) loadingItem.remove();

                let matched = medicalDB["default"];
                for (let key in medicalDB) {
                    if (key === "default") continue;
                    if (medicalDB[key].keywords.some(k => query.includes(k))) {
                        matched = medicalDB[key];
                        break;
                    }
                }

                const responseHtml = `
                    【VitaLens AI】：${matched.answer}
                    <span class="source-tag">🔍 RAG 實證來源：${matched.source}</span>
                `;
                appendChat(responseHtml, 'ai-text');
                addLog(`[零知識證明] 回傳解讀完畢，雲端快取即時銷毀`);
                
                // 3.2 會員點數增加
                updatePoints(10);
            }, 8000); // 模擬檢索時間
        }

        // 3.1 模擬高壓數據與 Nudge 效果
        function simulateHighPressure() {
            addLog("[第一方數據] WebSockets 連續 3 日採集到 HRV 偏低信號");
            document.getElementById('nudgeSection').style.display = 'block';
            addLog("[行為經濟學] 觸發 Nudge 機制：首頁自動更換為減壓專區");
        }

        // 3.2 模擬 CRM 貝氏 CLV 模型預測
        function simulateClvRetention() {
            addLog("[CRM運營] 啟動貝氏歸因模型分析用戶行為軌跡...");
            setTimeout(() => {
                const statusEl = document.getElementById('clvStatus');
                statusEl.innerText = "高價值活躍客群 (觸發尊榮方案推播)";
                statusEl.style.color = "#27AE60";
                addLog("[CRM模型] CLV 預測完成：歸類為高價值客群，更新行銷策略");
            }, 800);
        }

        // 4.1 Freemium 訂閱升級
        function triggerFreemiumUpgrade() {
            isPremium = true;
            document.getElementById('accountTier').innerText = "專業訂閱版 (NT$ 299/月)";
            document.getElementById('accountTier').style.color = "#27AE60";
            addLog("[商業邏輯] 用戶成功訂閱專業版，解除每日 RAG 諮詢限制");
        }

        // 4.2 公關危機模擬機制
        function triggerCrisis() {
            document.getElementById('crisisBanner').style.display = 'block';
            addLog("[危機應變] 啟動公關防火牆機制，發布極度透明公告");
            addLog("[技術阻斷] 全面重置 TLS 1.3 密鑰，強制落實誠信人格");
            triggerFreemiumUpgrade(); // 補償全體升級 6 個月
        }

        // 基礎輔助函式
        function appendChat(htmlContent, className, id = '') {
            const box = document.getElementById('chatMessages');
            const div = document.createElement('div');
            div.className = 'message';
            if (id) div.id = id;
            div.innerHTML = `<span class="${className}">${htmlContent}</span>`;
            box.appendChild(div);
            box.scrollTop = box.scrollHeight;
        }

        function addLog(text) {
            const list = document.getElementById('logList');
            const li = document.createElement('li');
            li.className = 'log-item';
            const now = new Date();
            const timeStr = `[${now.getHours().toString().padStart(2,'0')}:${now.getMinutes().toString().padStart(2,'0')}:${now.getSeconds().toString().padStart(2,'0')}]`;
            li.innerText = `${timeStr} ${text}`;
            list.insertBefore(li, list.firstChild);
        }

        function updatePoints(amount) {
            const el = document.getElementById('vitaPoints');
            let current = parseInt(el.innerText);
            el.innerText = current + amount;
            addLog(`[會員忠誠] 用戶完成互動，獲得 VitaPoints +${amount} pt`);
        }
    </script>
</body>
</html>
