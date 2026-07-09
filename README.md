<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCPU // CYBER PATROL OS PRO</title>
    <style>
        :root {
            --bg-base: #060814;
            --bg-card: #0F1322;
            --bg-input: #171E36;
            --text-main: #F1F5F9;
            --text-sub: #94A3B8;
            --border-neon: #0EA5E9;
            --border-dark: #1E293B;
            --accent-glow: rgba(14, 165, 233, 0.25);
            
            --signal-critical: #EF4444;
            --signal-warning: #F59E0B;
            --signal-success: #10B981;
            --signal-emergency: #A855F7;
        }

        /* 🌙 テーマ変数の切り替え用（ライトモード） */
        body.light-theme {
            --bg-base: #F8FAFC;
            --bg-card: #FFFFFF;
            --bg-input: #F1F5F9;
            --text-main: #0F172A;
            --text-sub: #475569;
            --border-dark: #CBD5E1;
            --border-neon: #0284C7;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Consolas', 'Courier New', monospace; }

        body {
            background-color: var(--bg-base);
            color: var(--text-main);
            padding-bottom: 60px;
            position: relative;
            transition: background 0.3s, color 0.3s;
        }

        /* ブラウン管風走査線エフェクト */
        body.scanlines::before {
            content: " "; display: block; position: fixed; top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            z-index: 9999; background-size: 100% 4px, 6px 100%; pointer-events: none;
        }

        /* 👑 ヘッダー & システムインジケーター */
        header {
            background-color: #02040A; border-bottom: 2px solid var(--border-neon);
            padding: 15px 24px; position: sticky; top: 0; z-index: 100;
            display: flex; justify-content: space-between; align-items: center;
        }

        header h1 { font-size: 1.1rem; color: #38BDF8; letter-spacing: 2px; }

        .sys-stats { display: flex; gap: 15px; align-items: center; font-size: 0.75rem; }
        .stat-badge { padding: 4px 10px; background: #111726; border: 1px solid var(--border-dark); border-radius: 4px; }
        .active-glow { animation: blink 1.5s infinite; color: var(--signal-success); }

        @keyframes blink { 0%, 100% { opacity: 0.4; } 50% { opacity: 1; } }
        @keyframes emergency-flash { 0%, 100% { background-color: var(--bg-base); } 50% { background-color: rgba(239, 68, 68, 0.3); } }
        body.emergency-alert { animation: emergency-flash 0.5s infinite; }

        /* 💥 ブートアニメーション */
        #boot-screen {
            position: fixed; top:0; left:0; width:100vw; height:100vh; background:#02040A;
            z-index: 10000; display: flex; flex-direction: column; justify-content: center; align-items: center;
            color: var(--border-neon); font-size: 1rem;
        }
        .progress-bar-container { width: 300px; height: 4px; background: #111726; margin-top: 15px; border-radius: 2px; overflow: hidden; }
        .progress-bar-fill { width: 0%; height: 100%; background: var(--border-neon); transition: width 0.1s linear; }

        /* 💻 メイングリッドレイアウト */
        .layout-grid {
            display: grid; grid-template-columns: 1fr 280px; max-width: 1400px; margin: 20px auto; gap: 20px; padding: 0 20px;
        }

        /* 🔍 検索マトリクス & フィルター */
        .control-panel { background: var(--bg-card); padding: 15px; border-radius: 6px; border: 1px solid var(--border-dark); margin-bottom: 20px; }
        .filter-row { display: flex; gap: 10px; margin-top: 10px; flex-wrap: wrap; }
        .btn-mini { padding: 5px 10px; font-size: 0.75rem; background: #131A30; border: 1px solid var(--border-dark); color: var(--text-main); cursor: pointer; border-radius: 3px; }
        .btn-mini.active, .btn-mini:hover { background: var(--border-neon); color: #02040A; }

        .search-card {
            background-color: var(--bg-card); border-radius: 6px; padding: 12px; margin-bottom: 10px; border: 1px solid var(--border-dark);
        }
        .sns-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 6px; margin-top: 8px; }
        .sns-btn { text-align: center; padding: 6px 0; font-size: 0.75rem; text-decoration: none; color: #fff; background: #1E293B; border-radius: 4px; font-weight: bold; }
        .sns-btn:hover { background: var(--border-neon); color: #02040A; }

        /* 🛠️ サイバーユーティリティツール */
        .util-box { background: var(--bg-card); border: 1px solid var(--border-dark); padding: 15px; border-radius: 6px; margin-bottom: 15px; }
        .util-title { font-size: 0.8rem; color: #38BDF8; font-weight: bold; margin-bottom: 10px; border-bottom: 1px solid var(--border-dark); padding-bottom: 4px; }
        .util-input { width: 100%; padding: 8px; font-size: 0.8rem; background: var(--bg-input); border: 1px solid var(--border-dark); color: #fff; margin-bottom: 8px; border-radius: 4px; }

        /* 📊 スプレッドシート枠 & クエスト */
        .frame-wrapper { background: #000; border: 1px solid var(--border-neon); border-radius: 6px; padding: 5px; height: 450px; }
        iframe { width: 100%; height: 100%; border: none; }

        /* 🔒 パニックボタン & クイックコピー */
        .panic-btn { background: var(--signal-critical) !important; color: white !important; font-weight: bold; }
        .emoji-pool { display: flex; gap: 8px; margin-top: 5px; }
        .emoji-btn { font-size: 1.1rem; cursor: pointer; background: none; border: none; }

        /* 🚨 モーダル調整 */
        .modal { display: none; position: fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.85); z-index:2000; align-items:center; justify-content:center; }
        .modal.active { display: flex; }
        .modal-content { background:#0B0F19; border:2px solid var(--border-neon); padding:20px; border-radius:6px; width:90%; max-width:500px; }
        .form-group { margin-bottom: 12px; }
        .form-group label { display:block; font-size:0.75rem; color:var(--text-sub); margin-bottom:4px; }
        .form-group input, .form-group select, .form-group textarea { width:100%; padding:8px; background:var(--bg-input); border:1px solid var(--border-dark); color:#fff; border-radius:4px; }

        .post-trigger-btn { position: fixed; bottom: 20px; right: 20px; width: 50px; height: 50px; background: #02040A; color: var(--border-neon); border: 2px solid var(--border-neon); border-radius: 50%; font-size: 1rem; cursor: pointer; display:flex; align-items:center; justify-content:center; text-decoration:none; z-index: 1000; }
        
        .tabs { display: flex; gap: 4px; margin-bottom: 10px; overflow-x: auto; }
        .tab-btn { padding: 8px 12px; font-size: 0.75rem; background: #090D1A; border: 1px solid var(--border-dark); color: var(--text-sub); cursor: pointer; white-space: nowrap; }
        .tab-btn.active { background: rgba(14, 165, 233, 0.1); border-color: var(--border-neon); color: #38BDF8; }
        .category-container { display: none; }
        .category-container.active { display: block; }
    </style>
</head>
<body class="scanlines">

<!-- 💥 166. サイバーブートアニメーション -->
<div id="boot-screen">
    <div id="boot-text">INITIALIZING CCPU SECURITY CORE v2.0.26...</div>
    <div class="progress-bar-container"><div class="progress-bar-fill" id="boot-progress"></div></div>
    <button class="btn-mini" style="margin-top:20px" onclick="skipBoot()">SKIP_INTRO</button>
</div>

<header>
    <h1>CCPU // PATROL OS PRO <span style="font-size:0.6rem; color:var(--text-sub);">v2.0.26-STABLE</span></h1>
    <div class="sys-stats">
        <!-- 👥 72. オンラインアナリスト、51. 通報カウンター、26. 定期巡回リマインダー -->
        <div class="stat-badge">AGENTS: <span class="active-glow" id="agent-count">12</span></div>
        <div class="stat-badge">TOTAL_REPORTS: <span id="global-counter" style="color:#38BDF8; font-weight:bold;">142</span></div>
        <div class="stat-badge" id="timer-display" style="color:var(--signal-warning);">NEXT_SYNC: 45:00</div>
        <button class="btn-mini" onclick="toggleTheme()">🎨 THEME</button>
    </div>
</header>

<div class="layout-grid">
    <!-- LeftSide: メイン監視・検索操縦盤 -->
    <main>
        <!-- 🔍 コントロールセンター（フィルター・時間軸） -->
        <div class="control-panel">
            <div style="font-size:0.8rem; font-weight:bold; color:var(--border-neon);">■ SEARCH PARAMETER GENERATOR</div>
            <!-- 1. プラットフォーム別 / 2. 時間軸指定 -->
            <div class="filter-row">
                <span>プラットフォーム指定:</span>
                <button class="btn-mini active" onclick="setPlatform('all')">ALL</button>
                <button class="btn-mini" onclick="setPlatform('x')">X (Twitter)</button>
                <button class="btn-mini" onclick="setPlatform('insta')">Instagram</button>
                <button class="btn-mini" onclick="setPlatform('tiktok')">TikTok</button>
            </div>
            <div class="filter-row" style="margin-top: 5px;">
                <span>時間軸ターゲット:</span>
                <button class="btn-mini active" onclick="setTimeframe('all')">全期間</button>
                <button class="btn-mini" onclick="setTimeframe('24h')">過去24時間以内 (Live)</button>
                <button class="btn-mini" onclick="setTimeframe('week')">今週分</button>
                <label style="font-size:0.75rem; margin-left:15px;"><input type="checkbox" id="and-or-toggle"> 複数条件AND検索</label>
            </div>
        </div>

        <!-- 3. 新規隠語アラート / 28. 最新手口ポップアップ -->
        <div class="util-box" style="border-color: var(--signal-warning); background: rgba(245,158,11,0.05);">
            <div class="util-title" style="color:var(--signal-warning); border-color:var(--signal-warning);">⚠️ 2026 CRITICAL ALERTS / EMERGENCY WORD</div>
            <div style="font-size:0.8rem; line-height:1.4;">
                <strong>【2026年最新隠語アラート】:</strong> 「タスク副業」「動画評価ずるずる」等のタスク詐欺求人が急増中。<br>
                <strong>【緊急指令】:</strong> 23:00〜4:00（深夜帯モード自動オンエリア）は「叩き」「臨航」等の強盗実行役募集の巡回を最優先せよ。
            </div>
        </div>

        <!-- パトロールマトリクス -->
        <div class="tabs">
            <button class="tab-btn active" onclick="switchCategory('cat1')">01. 強盗・実行犯</button>
            <button class="tab-btn" onclick="switchCategory('cat2')">02. 特殊詐欺・道具</button>
            <button class="tab-btn" onclick="switchCategory('cat3')">03. 秘匿誘導・移動</button>
            <button class="tab-btn" onclick="switchCategory('cat4')">04. カスタム保存</button>
        </div>

        <div id="cat1" class="category-container active"><div id="grid-cat1"></div></div>
        <div id="cat2" class="category-container"><div id="grid-cat2"></div></div>
        <div id="cat3" class="category-container"><div id="grid-cat3"></div></div>
        <!-- 4. カスタムキーワード追加ブックマーク -->
        <div id="cat4" class="category-container">
            <div class="util-box">
                <div class="util-title">マイ・ブックマークワード</div>
                <div style="display:flex; gap:5px; margin-bottom:10px;">
                    <input type="text" id="custom-word-input" class="util-input" style="margin:0;" placeholder="よく巡回するワードを入力">
                    <button class="btn-mini" onclick="addCustomWord()">追加</button>
                </div>
                <div id="custom-words-pool" class="filter-row"></div>
            </div>
        </div>

        <!-- 📊 LIVE FEED SECTION -->
        <div style="margin: 20px 0 10px 0; font-size:0.85rem; font-weight:bold; color:var(--border-neon);">■ LIVE INCIDENT FEED (CLOUD REPLICATION)</div>
        <div class="frame-wrapper">
            <!-- 💡 ここにGoogleスプレッドシートのウェブ公開URLを反映 -->
            <iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vT1Z_GvQ9F3E-YourID/pubhtml?widget=true&amp;headers=false"></iframe>
        </div>
    </main>

    <!-- RightSide: セキュリティユーティリティ・アナリストコンソール -->
    <aside>
        <!-- 71. アナリストデータ & 95. デイリー任務 -->
        <div class="util-box" style="border-color: var(--border-neon);">
            <div class="util-title">AGENTS CONSOLE</div>
            <div style="font-size:0.8rem; margin-bottom:10px;">
                CODE: <span style="color:#38BDF8; font-weight:bold;" id="agent-name">ANALYST_X</span><br>
                RANK: <span style="color:var(--signal-success); font-weight:bold;">RANK: CYBER MASTER</span>
            </div>
            <div style="background:#111726; padding:8px; border-radius:4px; font-size:0.75rem;">
                <div style="color:var(--signal-warning); font-weight:bold; margin-bottom:3px;">🎯 本日のデイリー任務</div>
                <div id="daily-quest">薬物・強盗系隠語を5回以上巡回せよ [0/5]</div>
            </div>
        </div>

        <!-- 108. パニックボタン & 5. 絵文字クイックコピー -->
        <div class="util-box">
            <div class="util-title">QUICK DEFENSE / EMOJI</div>
            <button class="util-input panic-btn" onclick="activatePanic()">🚨 PANIC BUTTON (Esc)</button>
            <div class="util-title" style="margin-top:10px; border:none;">絵文字クイックコピー</div>
            <div class="emoji-pool">
                <button class="emoji-btn" onclick="copyEmoji('🥦')">🥦</button>
                <button class="emoji-btn" onclick="copyEmoji('🍧')">🍧</button>
                <button class="emoji-btn" onclick="copyEmoji('💰❌')">💰❌</button>
                <button class="emoji-btn" onclick="copyEmoji('🪓')">🪓</button>
            </div>
        </div>

        <!-- 9. URLクレンジング & 8. アカウント逆引き -->
        <div class="util-box">
            <div class="util-title">URL CLEANSER</div>
            <input type="text" id="dirty-url" class="util-input" placeholder="不審なURLをペースト" oninput="cleanURL()">
            <div id="clean-url-result" style="font-size:0.7rem; color:var(--signal-success); word-break:break-all;"></div>
        </div>

        <div class="util-box">
            <div class="util-title">ID REVERSE SEARCH</div>
            <input type="text" id="search-id" class="util-input" placeholder="悪質アカウントのID">
            <button class="btn-mini" style="width:100%" onclick="reverseSearchID()">主要4プラットフォーム逆引き</button>
        </div>

        <!-- 24. シグナル・テレグラムID抽出器 -->
        <div class="util-box">
            <div class="util-title">CONTACT ID EXTRACTOR</div>
            <textarea id="extractor-text" class="util-input" rows="3" placeholder="不審な投稿テキストをそのまま張り付け" oninput="extractIDs()"></textarea>
            <div id="extractor-result" style="font-size:0.75rem; color:#38BDF8;"></div>
        </div>

        <!-- 191. アナリスト個人メモ帳エリア -->
        <div class="util-box">
            <div class="util-title">LOCAL MEMO PAD</div>
            <textarea id="local-memo" class="util-input" rows="4" placeholder="パトロール中の備忘録・引き継ぎ事項（ブラウザに自動保存）" oninput="saveMemo()"></textarea>
        </div>
    </aside>
</div>

<!-- 報告用フローティングボタン -->
<a href="https://docs.google.com/forms/d/e/1FAIpQLSfs_YourFormID/viewform" target="_blank" class="post-trigger-btn">報告</a>

<script>
    // 166. ブートアニメーションシミュレーター
    let progress = 0;
    const progressFill = document.getElementById('boot-progress');
    const bootScreen = document.getElementById('boot-screen');
    
    const bootTimer = setInterval(() => {
        progress += 4;
        if(progressFill) progressFill.style.width = progress + '%';
        if (progress >= 100) {
            clearInterval(bootTimer);
            if(bootScreen) bootScreen.style.display = 'none';
            initSystem();
        }
    }, 50);

    function skipBoot() {
        clearInterval(bootTimer);
        if(bootScreen) bootScreen.style.display = 'none';
        initSystem();
    }

    // グローバルパトロールパラメータ
    let currentPlatform = 'all';
    let currentTimeframe = 'all';
    let questCount = 0;

    const keywordData = {
        cat1: [
            { word: "叩き 案件", desc: "強盗・緊縛強盗の最凶隠語", ambiguous: ["たたき案件", "タタキ案件"] },
            { word: "タコ 案件", desc: "現場仕事を装った強盗募集", ambiguous: ["たこ案件", "蛸案件"] },
            { word: "臨航 案件", desc: "強盗現場への移動・車両指示", ambiguous: ["りんこう案件", "臨港案件"] }
        ],
        cat2: [
            { word: "受け子 募集", desc: "特殊詐欺の現金回収係", ambiguous: ["うけこ募集"] },
            { word: "出し子 募集", desc: "ATM不正現金引き出し役", ambiguous: ["だしこ募集"] },
            { word: "口座買取", desc: "犯罪用インフラ口座の調達", ambiguous: ["口座売買"] }
        ],
        cat3: [
            { word: "詳細はシグナル", desc: "暗号化アプリSignalへの誘導", ambiguous: ["詳細はSignal"] },
            { word: "シグナル 移行", desc: "足跡を消すための定番文句", ambiguous: ["Signal移行"] },
            { word: "タスク副業", desc: "2026年急増動画評価型タスク詐欺", ambiguous: ["タスク案件", "動画評価バイト"] }
        ]
    };

    function initSystem() {
        renderMatrix();
        loadMemo();
        loadCustomWords();
        checkMidnightMode();
        setInterval(checkMidnightMode, 60000); // 1分ごとに深夜帯判定
        
        // 72. アナリスト数のシミュレーション変動
        setInterval(() => {
            const count = Math.floor(Math.random() * 5) + 10;
            document.getElementById('agent-count').innerText = count;
        }, 10000);
    }

    function setPlatform(p) { currentPlatform = p; renderMatrix(); }
    function setTimeframe(t) { currentTimeframe = t; renderMatrix(); }

    // 🔍 1. プラットフォームフィルター / 2. 時間軸指定 / 6. あいまい検索リンク / 7. 地域限定検索
    function renderMatrix() {
        for (let cat in keywordData) {
            const targetGrid = document.getElementById('grid-' + cat);
            if (!targetGrid) continue;
            targetGrid.innerHTML = '';

            keywordData[cat].forEach(item => {
                const card = document.createElement('div');
                card.className = 'search-card';

                // 時間軸パラメータのモック生成
                let timeParam = '';
                if (currentTimeframe === '24h') timeParam = ' since:2026-07-08'; // 2026年リアルタイム軸
                if (currentTimeframe === 'week') timeParam = ' within_time:7d';

                const baseWord = item.word + timeParam;
                const encWord = encodeURIComponent(baseWord);

                // 6. あいまいワードの包含
                let ambiguousQuery = encodeURIComponent(item.word + " OR " + item.ambiguous.join(" OR "));

                card.innerHTML = `
                    <div style="display:flex; justify-content:space-between; font-size:0.8rem; border-bottom:1px solid var(--border-dark); padding-bottom:4px;">
                        <strong>${item.word}</strong>
                        <span style="font-size:0.7rem; color:var(--text-sub);">${item.desc}</span>
                    </div>
                    <div class="sns-grid">
                        ${currentPlatform === 'all' || currentPlatform === 'x' ? `<a href="https://x.com/search?q=${ambiguousQuery}${timeParam}&f=live" target="_blank" class="sns-btn" onclick="countQuest()">X (Live)</a>` : ''}
                        ${currentPlatform === 'all' || currentPlatform === 'insta' ? `<a href="https://www.instagram.com/explore/tags/${encodeURIComponent(item.word.replace(/\s+/g, ''))}/" target="_blank" class="sns-btn" onclick="countQuest()">Insta</a>` : ''}
                        ${currentPlatform === 'all' || currentPlatform === 'tiktok' ? `<a href="https://www.tiktok.com/search?q=${encWord}" target="_blank" class="sns-btn" onclick="countQuest()">TikTok</a>` : ''}
                        <a href="https://x.com/search?q=${encodeURIComponent(item.word + " 東京 OR 大阪")}" target="_blank" class="sns-btn" style="background:#047857;">🗺️ 地域限定</a>
                    </div>
                `;
                targetGrid.appendChild(card);
            });
        }
    }

    // 4. カスタムキーワード追加ボタン
    function addCustomWord() {
        const input = document.getElementById('custom-word-input');
        let words = JSON.parse(localStorage.getItem('ccpu_custom_words') || '[]');
        if (input.value && !words.includes(input.value)) {
            words.push(input.value);
            localStorage.setItem('ccpu_custom_words', JSON.stringify(words));
            input.value = '';
            loadCustomWords();
        }
    }

    function loadCustomWords() {
        const pool = document.getElementById('custom-words-pool');
        if(!pool) return;
        pool.innerHTML = '';
        let words = JSON.parse(localStorage.getItem('ccpu_custom_words') || '[]');
        words.forEach(word => {
            const btn = document.createElement('button');
            btn.className = 'btn-mini';
            btn.innerText = `🔍 ${word}`;
            btn.onclick = () => window.open(`https://x.com/search?q=${encodeURIComponent(word)}&f=live`);
            pool.appendChild(btn);
        });
    }

    // 5. 絵文字クイックコピー
    function copyEmoji(emoji) {
        navigator.clipboard.writeText(emoji);
        alert(`[SYS] クリップボードにコピーしました: ${emoji}`);
    }

    // 8. アカウントID逆引き検索
    function reverseSearchID() {
        const id = document.getElementById('search-id').value.replace('@', '');
        if(!id) return;
        window.open(`https://x.com/${id}`, '_blank');
        window.open(`https://www.instagram.com/${id}`, '_blank');
        window.open(`https://www.tiktok.com/@${id}`, '_blank');
    }

    // 9. URLクレンジング機能
    function cleanURL() {
        const dirty = document.getElementById('dirty-url').value;
        try {
            const url = new URL(dirty);
            url.search = ''; // パラメータを全消去 (?s=... 等をクレンジング)
            document.getElementById('clean-url-result').innerText = `整形済み安全リンク:\n${url.href}`;
        } catch(e) {
            document.getElementById('clean-url-result').innerText = '有効なURLを入力してください。';
        }
    }

    // 10. 深夜帯パトロールモード（23:00 - 4:00に警告発動）
    function checkMidnightMode() {
        const hour = new Date().getHours();
        if (hour >= 23 || hour < 4) {
            document.body.classList.add('emergency-alert');
            document.getElementById('timer-display').innerText = "⚠️ 深夜警戒モード稼働中";
        } else {
            document.body.classList.remove('emergency-alert');
            document.getElementById('timer-display').innerText = "SYSTEM: ONLINE";
        }
    }

    // 24. シグナル・テレグラムID抽出器
    function extractIDs() {
        const text = document.getElementById('extractor-text').value;
        const telegramRegex = /(?:t\.me\/|@)([a-zA-Z0-9_]{5,32})/g;
        let matches = [];
        let match;
        while ((match = telegramRegex.exec(text)) !== null) {
            matches.push(match[1]);
        }
        document.getElementById('extractor-result').innerText = matches.length ? `検知連絡先ID: ${matches.join(', ')}` : '連絡先IDは検知されませんでした。';
    }

    // 108. パニックボタン (Escキー連動)
    function activatePanic() {
        window.location.href = 'https://news.google.com/';
    }
    window.addEventListener('keydown', (e) => { if (e.key === 'Escape') activatePanic(); });

    // 191. メモ帳ローカル自動保存
    function saveMemo() {
        const txt = document.getElementById('local-memo').value;
        localStorage.setItem('ccpu_local_memo', txt);
    }
    function loadMemo() {
        const saved = localStorage.getItem('ccpu_local_memo');
        if(saved) document.getElementById('local-memo').value = saved;
    }

    // 95. デイリー任務カウンター
    function countQuest() {
        questCount++;
        const questBox = document.getElementById('daily-quest');
        if (questCount >= 5) {
            questBox.innerHTML = '🎉 <span style="color:var(--signal-success);">任務完了！サイバー空間が安全になりました。</span>';
        } else {
            questBox.innerText = `薬物・強盗系隠語を5回以上巡回せよ [${questCount}/5]`;
        }
    }

    function switchCategory(catId) {
        document.querySelectorAll('.category-container').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(t => t.classList.remove('active'));
        document.getElementById(catId).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    function toggleTheme() {
        document.body.classList.toggle('light-theme');
    }
</script>
</body>
</html>
