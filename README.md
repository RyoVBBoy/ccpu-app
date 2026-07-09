<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCPU サイバーパトロール・インシデントハブ</title>
    <style>
        /* 🌌 サイバーセキュリティをイメージしたディープダーク＆ネオンカラー構成 */
        :root {
            --bg-base: #0B0F19;      /* 漆黒・空間のベースカラー */
            --bg-card: #161F30;      /* サイバーブルーグレー（カード背景） */
            --text-main: #E2E8F0;    /* サイバーホワイト（メイン文字） */
            --text-sub: #94A3B8;     /* ローライトグレー */
            --border-neon: #0EA5E9;  /* ネオンブルー（境界・アクティブ） */
            --accent-glow: rgba(14, 165, 233, 0.15);
            
            /* 深刻度・ジャッジシグナル */
            --signal-critical: #EF4444; /* クリティカル（赤） */
            --signal-warning: #F59E0B;  /* ワーニング（橙） */

            /* 各SNSボタンの専用カラー */
            --color-x: #000000;
            --color-insta: #E1306C;
            --color-tiktok: #000000;
            --color-yay: #38BDF8; /* サイバー感に合わせたライトブルー */
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Consolas', 'Courier New', 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background-color: var(--bg-base);
            color: var(--text-main);
            padding-bottom: 80px;
        }

        /* 👑 サイバーセキュリティ・ヘッダー */
        header {
            background-color: #020617;
            border-bottom: 2px solid var(--border-neon);
            color: white;
            padding: 15px;
            position: sticky;
            top: 0;
            z-index: 100;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 0 15px rgba(14, 165, 233, 0.3);
        }

        header h1 {
            font-size: 1.1rem;
            font-weight: bold;
            letter-spacing: 2px;
            color: #38BDF8;
            text-shadow: 0 0 8px rgba(56, 189, 248, 0.6);
        }

        .header-status {
            font-size: 0.75rem;
            border: 1px solid #22C55E;
            color: #22C55E;
            background-color: rgba(34, 197, 94, 0.1);
            padding: 4px 12px;
            border-radius: 4px;
            font-weight: bold;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { opacity: 0.6; }
            50% { opacity: 1; }
            100% { opacity: 0.6; }
        }

        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 0 15px;
        }

        /* SECTION TITLE */
        .section-title {
            font-size: 1rem;
            color: #38BDF8;
            margin: 30px 0 15px 0;
            letter-spacing: 1px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .section-title::before {
            content: "▶";
            font-size: 0.8rem;
        }

        /* 📑 一括検索カテゴリ切り替えタブ */
        .tabs {
            display: flex;
            gap: 6px;
            margin-bottom: 15px;
            overflow-x: auto;
            padding-bottom: 5px;
        }

        .tab-btn {
            flex: 1;
            min-width: 120px;
            padding: 10px 6px;
            background-color: #1E293B;
            border: 1px solid #334155;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            color: var(--text-sub);
            transition: all 0.2s ease;
            text-align: center;
            font-size: 0.8rem;
        }

        .tab-btn.active {
            background-color: rgba(14, 165, 233, 0.15);
            border-color: var(--border-neon);
            color: #38BDF8;
            text-shadow: 0 0 5px rgba(56, 189, 248, 0.5);
        }

        /* キーワード一括検索セクション */
        .category-container {
            display: none;
        }

        .category-container.active {
            display: block;
        }

        /* 隠語検索カード */
        .search-card {
            background-color: var(--bg-card);
            border-radius: 6px;
            padding: 14px;
            margin-bottom: 12px;
            border: 1px solid #1E293B;
        }

        .search-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 10px;
            border-bottom: 1px solid #1E293B;
            padding-bottom: 4px;
        }

        .search-name {
            font-size: 0.95rem;
            font-weight: bold;
            color: var(--text-main);
        }

        .search-desc {
            font-size: 0.75rem;
            color: var(--text-sub);
        }

        /* SNSジャンプボタンのグリッド */
        .sns-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
        }

        .sns-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 8px 4px;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.8rem;
            border-radius: 4px;
            color: white;
            transition: all 0.15s ease;
            text-align: center;
            background-color: #1E293B;
            border: 1px solid #334155;
        }
        .sns-btn:hover {
            transform: translateY(-1px);
            border-color: var(--border-neon);
            box-shadow: 0 0 8px var(--accent-glow);
            color: #38BDF8;
        }

        /* ✉️ インシデント報告ログカード */
        .post-card {
            background-color: var(--bg-card);
            border-radius: 6px;
            padding: 16px;
            margin-bottom: 12px;
            border: 1px solid #1E293B;
            position: relative;
            transition: border-color 0.3s;
        }
        .post-card:hover {
            border-color: var(--border-neon);
            box-shadow: 0 0 10px var(--accent-glow);
        }

        /* アナリスト情報 */
        .post-user-info {
            display: flex;
            align-items: center;
            margin-bottom: 12px;
        }

        .avatar {
            width: 36px;
            height: 36px;
            background-color: #1E293B;
            border: 1px solid var(--border-neon);
            border-radius: 4px;
            margin-right: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #38BDF8;
            font-size: 0.75rem;
        }

        .user-meta {
            display: flex;
            flex-direction: column;
        }

        .user-name {
            font-size: 0.85rem;
            font-weight: bold;
            color: var(--text-main);
        }

        .post-time {
            font-size: 0.75rem;
            color: var(--text-sub);
        }

        /* アラートレベルバッジ */
        .judge-badge {
            position: absolute;
            top: 16px;
            right: 16px;
            padding: 3px 8px;
            border-radius: 4px;
            font-size: 0.7rem;
            font-weight: bold;
            letter-spacing: 1px;
        }
        .bg-critical { background-color: rgba(239, 68, 68, 0.15); border: 1px solid var(--signal-critical); color: #FCA5A5; }
        .bg-warning { background-color: rgba(245, 158, 11, 0.15); border: 1px solid var(--signal-warning); color: #FDE68A; }

        /* ログ本文 */
        .post-content {
            font-size: 0.85rem;
            white-space: pre-wrap;
            margin-bottom: 12px;
            word-break: break-all;
            color: #CBD5E1;
        }

        .post-link {
            display: inline-block;
            color: #38BDF8;
            text-decoration: none;
            font-size: 0.75rem;
            margin-top: 2px;
            word-break: break-all;
            border-bottom: 1px dashed #38BDF8;
        }

        /* 📊 コマンド・アクションバー */
        .action-bar {
            display: flex;
            border-top: 1px solid #1E293B;
            padding-top: 10px;
            color: var(--text-sub);
            font-size: 0.75rem;
            gap: 20px;
        }

        .action-item {
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .action-item:hover {
            color: var(--border-neon);
        }

        /* ➕ フローティング報告エントリーボタン */
        .post-trigger-btn {
            position: fixed;
            bottom: 25px;
            right: 25px;
            width: 52px;
            height: 52px;
            background-color: #020617;
            color: var(--border-neon);
            border: 2px solid var(--border-neon);
            border-radius: 6px;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 0 15px rgba(14, 165, 233, 0.4);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 200;
        }
        .post-trigger-btn:hover {
            background-color: var(--border-neon);
            color: #020617;
            box-shadow: 0 0 20px var(--border-neon);
        }

        /* 📝 報告入力モーダル */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-color: rgba(2, 6, 23, 0.85);
            z-index: 300;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(4px);
        }
        .modal.active { display: flex; }

        .modal-content {
            background: #0F172A;
            border: 2px solid var(--border-neon);
            width: 90%;
            max-width: 500px;
            border-radius: 8px;
            padding: 20px;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            font-weight: bold;
            color: #38BDF8;
        }

        .close-btn { cursor: pointer; color: var(--text-sub); }

        .form-group {
            margin-bottom: 12px;
        }
        .form-group label {
            display: block;
            font-size: 0.7rem;
            font-weight: bold;
            margin-bottom: 4px;
            color: var(--text-sub);
        }
        .form-group select, .form-group textarea, .form-group input {
            width: 100%;
            padding: 8px;
            background-color: #1E293B;
            border: 1px solid #334155;
            border-radius: 4px;
            font-size: 0.85rem;
            color: white;
        }

        .submit-btn {
            width: 100%;
            padding: 10px;
            background-color: var(--border-neon);
            color: #020617;
            border: none;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<header>
    <h1>CCPU // CYBER PATROL CORE</h1>
    <div class="header-status">SYSTEM ONLINE</div>
</header>

<div class="container">
    
    <div class="section-title">TARGET SEARCH TARGET MATRIX [一括外部検索]</div>
    
    <div class="tabs">
        <button class="tab-btn active" onclick="switchCategory('cat1')">1. 強盗・実行役</button>
        <button class="tab-btn" onclick="switchCategory('cat2')">2. 特殊詐欺・道具</button>
        <button class="tab-btn" onclick="switchCategory('cat3')">3. 秘匿誘導</button>
        <button class="tab-btn" onclick="switchCategory('cat4')">4. 薬物密売</button>
        <button class="tab-btn" onclick="switchCategory('cat5')">5. タスク詐欺</button>
    </div>

    <div id="cat1" class="category-container active"><div id="grid-cat1"></div></div>
    <div id="cat2" class="category-container"><div id="grid-cat2"></div></div>
    <div id="cat3" class="category-container"><div id="grid-cat3"></div></div>
    <div id="cat4" class="category-container"><div id="grid-cat4"></div></div>
    <div id="cat5" class="category-container"><div id="grid-cat5"></div></div>

    <div class="section-title">REAL-TIME INCIDENT FEED [内部共有ログ]</div>
    
    <div id="timeline">
        <div class="post-card">
            <span class="judge-badge bg-critical">CRITICAL [即通報]</span>
            <div class="post-user-info">
                <div class="avatar">AN-A</div>
                <div class="user-meta">
                    <span class="user-name">パトロールアナリスト Alpha</span>
                    <span class="post-time">今日 13:15 · 監視対象プラットフォームにて検知</span>
                </div>
            </div>
            <div class="post-content">「即日5万、詳細はシグナルへ。足跡つかないホワイト」という定型勧誘を検知。アカウント作成から48時間以内。完全に実行役（受け子）の募集。</div>
            <a href="#" target="_blank" class="post-link">🔗 該当ログの証拠ソースURLを開く</a>
            <div class="action-bar">
                <div class="action-item" onclick="alert('カウンターシグナルを同期しました')">🚨 通報連携完了</div>
                <div class="action-item">💬 解析ログコメント (0)</div>
            </div>
        </div>
    </div>

</div>

<button class="post-trigger-btn" onclick="toggleModal(true)">＋</button>

<div class="modal" id="postModal">
    <div class="modal-content">
        <div class="modal-header">
            <span>[ENTRY] インシデントログ報告</span>
            <span class="close-btn" onclick="toggleModal(false)">✕</span>
        </div>
        <form id="patrolForm" onsubmit="submitPost(event)">
            <div class="form-group">
                <label>検知ソースプラットフォーム</label>
                <select id="postSns">
                    <option value="主要SNS (X等)">主要SNS (X等)</option>
                    <option value="画像・動画配信系">画像・動画配信系</option>
                    <option value="匿名チャットプラットフォーム">匿名チャットプラットフォーム</option>
                </select>
            </div>
            <div class="form-group">
                <label>CCPUセキュリティジャッジ</label>
                <select id="postJudge">
                    <option value="bg-critical">CRITICAL [即通報]</option>
                    <option value="bg-warning">WARNING [要警戒]</option>
                </select>
            </div>
            <div class="form-group">
                <label>検知内容・不審点の詳細</label>
                <textarea id="postText" rows="4" placeholder="検知した内容、隠語、アカウントの特徴を入力" required></textarea>
            </div>
            <div class="form-group">
                <label>対象の証拠URL（任意）</label>
                <input type="url" id="postUrl" placeholder="https://...">
            </div>
            <button type="submit" class="submit-btn">ログをフィードへ射出</button>
        </form>
    </div>
</div>

<script>
    // キーワードマスターデータ
    const keywordData = {
        cat1: [
            { word: "叩き 案件", desc: "強盗・緊縛強盗の最凶隠語" },
            { word: "タコ 案件", desc: "現場仕事を装った強盗募集" },
            { word: "臨航 案件", desc: "強盗・窃盗現場への移動指示" },
            { word: "臨務 案件", desc: "強盗実行・破壊工作の隠語" },
            { word: "造作 案件", desc: "強盗前の下見・リサーチ役" },
            { word: "引越し 荷揚げ", desc: "深夜の窃盗・空き巣実行役" },
            { word: "#高額案件", desc: "闇バイト定番ハッシュタグ" },
            { word: "#ホワイト案件", desc: "安全を装う規約違反募集" }
        ],
        cat2: [
            { word: "受け子 募集", desc: "特殊詐欺の現金・カード回収係" },
            { word: "出し子 募集", desc: "ATMでの不正現金引き出し役" },
            { word: "口座買取", desc: "犯罪用インフラ（口座）の調達" },
            { word: "シム買取", desc: "飛ばし携帯・SIMカードの売買" },
            { word: "郵送案件", desc: "騙し取ったカード等の転送係" }
        ],
        cat3: [
            { word: "詳細はシグナル", desc: "暗号化アプリSignalへの誘導" },
            { word: "シグナル 移行", desc: "足跡を消すための定番文句" },
            { word: "テレグラム 案件", desc: " Telegramでの指示待ち要員募集" },
            { word: "裏に回れる方", desc: "秘匿連絡への移行が条件" }
        ],
        cat4: [
            { word: "🥦 手渡し", desc: "大麻密売（ブロッコリー絵文字）" },
            { word: "野菜 手押し", desc: "大麻の対面密売" },
            { word: "🍧 冷茶", desc: "覚醒剤密売（かき氷絵文字）" },
            { word: "アイス チャリ", desc: "覚醒剤の自転車配達" },
            { word: "🍯 リキッド", desc: "液体大麻（ハチミツ絵文字）" }
        ],
        cat5: [
            { word: "タスク副業", desc: "いいねや動画評価を騙る最新詐欺" },
            { word: "動画評価 副業", desc: "最初に数千円払い信用させる手口" },
            { word: "初心者ずるずる", desc: "SNS内でのカモ探しワード" },
            { word: "裏垢女子 副業", desc: "botを用いた悪質サイト誘導" }
        ]
    };

    window.onload = function() {
        for (let cat in keywordData) {
            const targetGrid = document.getElementById('grid-' + cat);
            keywordData[cat].forEach(item => {
                const card = document.createElement('div');
                card.className = 'search-card';
                const encWord = encodeURIComponent(item.word);
                
                const urlX = `https://x.com/search?q=${encWord}&f=live`;
                const urlInsta = `https://www.instagram.com/explore/tags/${encWord.replace(/%20/g, '')}/`;
                const urlTiktok = `https://www.tiktok.com/search?q=${encWord}`;
                const urlYay = `https://yay.space/search/posts?query=${encWord}`;

                card.innerHTML = `
                    <div class="search-header">
                        <span class="search-name">${item.word}</span>
                        <span class="search-desc">${item.desc}</span>
                    </div>
                    <div class="sns-grid">
                        <a href="${urlX}" target="_blank" class="sns-btn">X</a>
                        <a href="${urlInsta}" target="_blank" class="sns-btn">Insta</a>
                        <a href="${urlTiktok}" target="_blank" class="sns-btn">TikTok</a>
                        <a href="${urlYay}" target="_blank" class="sns-btn">匿名P</a>
                    </div>
                `;
                targetGrid.appendChild(card);
            });
        }
    };

    function switchCategory(catId) {
        document.querySelectorAll('.category-container').forEach(c => c.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(t => t.classList.remove('active'));
        document.getElementById(catId).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    function toggleModal(show) {
        const modal = document.getElementById('postModal');
        if (show) modal.classList.add('active');
        else modal.classList.remove('active');
    }

    function submitPost(e) {
        e.preventDefault();
        const sns = document.getElementById('postSns').value;
        const judgeClass = document.getElementById('postJudge').value;
        const judgeText = document.getElementById('postJudge').options[document.getElementById('postJudge').selectedIndex].text;
        const text = document.getElementById('postText').value;
        const url = document.getElementById('postUrl').value;

        const timeline = document.getElementById('timeline');
        const newPost = document.createElement('div');
        newPost.className = 'post-card';
        let linkHTML = url ? `<a href="${url}" target="_blank" class="post-link">🔗 該当ログの証拠ソースURLを開く</a>` : '';

        newPost.innerHTML = `
            <span class="judge-badge ${judgeClass}">${judgeText}</span>
            <div class="post-user-info">
                <div class="avatar">AN-NEW</div>
                <div class="user-meta">
                    <span class="user-name">CCPUパトロールアナリスト</span>
                    <span class="post-time">たった今 · ${sns}にて検知</span>
                </div>
            </div>
            <div class="post-content">${text}</div>
            ${linkHTML}
            <div class="action-bar">
                <div class="action-item">🚨 通報連携完了</div>
                <div class="action-item">💬 解析ログコメント (0)</div>
            </div>
        `;
        timeline.insertBefore(newPost, timeline.firstChild);
        document.getElementById('patrolForm').reset();
        toggleModal(false);
    }
</script>
</body>
</html>
