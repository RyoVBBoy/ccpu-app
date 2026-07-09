<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCPU サイバーパトロール・特化型検索ハブ</title>
    <style>
        /* Pro Finder（安否確認アプリ）を意識した、信頼感のあるディープブルー */
        :root {
            --primary-color: #1A365D; /* ヘッダー・メイン */
            --secondary-color: #2B6CB0; /* アクティブ・ボタン */
            --bg-color: #F7FAFC;
            --card-bg: #FFFFFF;
            --text-color: #2D3748;
            --border-color: #E2E8F0;
            
            /* SNS毎のブランドカラー */
            --color-x: #000000;
            --color-insta: #E1306C;
            --color-tiktok: #000000;
            --color-yay: #FFCC00; /* Yay! のイメージカラー（黄色・文字は黒に） */
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            line-height: 1.5;
            padding-bottom: 60px;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            text-align: center;
            padding: 25px 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        header h1 {
            font-size: 1.6rem;
            margin-bottom: 5px;
            letter-spacing: 1px;
        }

        header p {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .container {
            max-width: 1000px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .intro-box {
            background-color: var(--card-bg);
            border-left: 5px solid #E53E3E;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 4px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            font-size: 0.85rem;
        }

        /* カテゴリ切り替えタブ */
        .tabs {
            display: flex;
            gap: 8px;
            margin-bottom: 25px;
            overflow-x: auto;
            padding-bottom: 5px;
        }

        .tab-btn {
            flex: 1;
            min-width: 130px;
            padding: 12px 8px;
            background-color: #E2E8F0;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            color: var(--primary-color);
            transition: all 0.2s ease;
            text-align: center;
            font-size: 0.85rem;
        }

        .tab-btn.active {
            background-color: var(--secondary-color);
            color: white;
        }

        /* キーワードセクション */
        .category-container {
            display: none;
        }

        .category-container.active {
            display: block;
        }

        /* 隠語ごとのカードスタイル */
        .keyword-card {
            background-color: var(--card-bg);
            border-radius: 8px;
            padding: 18px;
            margin-bottom: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            border: 1px solid var(--border-color);
        }

        .keyword-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 12px;
            border-bottom: 2px solid #EDF2F7;
            padding-bottom: 6px;
        }

        .keyword-name {
            font-size: 1.1rem;
            font-weight: bold;
            color: var(--primary-color);
        }

        .keyword-desc {
            font-size: 0.8rem;
            color: #718096;
            background-color: #EDF2F7;
            padding: 2px 8px;
            border-radius: 4px;
        }

        /* SNSジャンプボタンのグリッド */
        .sns-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        @media (max-width: 600px) {
            .sns-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }

        .sns-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 10px 5px;
            text-decoration: none;
            font-weight: bold;
            font-size: 0.85rem;
            border-radius: 6px;
            color: white;
            transition: transform 0.15s ease, opacity 0.2s;
            text-align: center;
        }

        .sns-btn:hover {
            transform: translateY(-2px);
            opacity: 0.9;
        }

        /* 各SNSボタンの専用カラー */
        .btn-x { background-color: var(--color-x); }
        .btn-insta { background-color: var(--color-insta); }
        .btn-tiktok { background-color: var(--color-tiktok); }
        .btn-yay { background-color: var(--color-yay); color: #1A202C; } /* Yayは視認性のため黒文字 */

        footer {
            text-align: center;
            margin-top: 40px;
            font-size: 0.8rem;
            color: #718096;
        }
    </style>
</head>
<body>

<header>
    <h1>CCPU パトロール検索ハブ</h1>
    <p>最新隠語・絵文字対応版 / 5大警戒カテゴリ一括監視ツール</p>
</header>

<div class="container">
    <div class="intro-box">
        🚨 <strong>CCPUパトロールマニュアル:</strong> 上部のタブで目的の犯罪カテゴリを切り替えます。各隠語の右側にある「SNSボタン」を押すと、そのSNS内で該当ワードが入力された検索結果（Xは最新順、Yay!は投稿タイムライン）へ直接ジャンプします。
    </div>

    <!-- カテゴリ切り替えタブ -->
    <div class="tabs">
        <button class="tab-btn active" onclick="switchCategory('cat1')">1. 強盗・実行役</button>
        <button class="tab-btn" onclick="switchCategory('cat2')">2. 特殊詐欺・道具</button>
        <button class="tab-btn" onclick="switchCategory('cat3')">3. 秘匿アプリ誘導</button>
        <button class="tab-btn" onclick="switchCategory('cat4')">4. 薬物密売</button>
        <button class="tab-btn" onclick="switchCategory('cat5')">5. 副業・タスク詐欺</button>
    </div>

    <!-- ==================== 1. 強盗・実行役募集 ==================== -->
    <div id="cat1" class="category-container active">
        <div id="grid-cat1"></div>
    </div>

    <!-- ==================== 2. 特殊詐欺・道具調達 ==================== -->
    <div id="cat2" class="category-container">
        <div id="grid-cat2"></div>
    </div>

    <!-- ==================== 3. 秘匿アプリ誘導 ==================== -->
    <div id="cat3" class="category-container">
        <div id="grid-cat3"></div>
    </div>

    <!-- ==================== 4. 薬物密売 ==================== -->
    <div id="cat4" class="category-container">
        <div id="grid-cat4"></div>
    </div>

    <!-- ==================== 5. 副業・タスク詐欺 ==================== -->
    <div id="cat5" class="category-container">
        <div id="grid-cat5"></div>
    </div>
</div>

<footer>
    <p>© 2026 CCPU Cyber Crime Prevention Unit. All Rights Reserved.</p>
</footer>

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
            { word: "#ホワイト案件", desc: "安全を装う規約違反募集" },
            { word: "日当10万", desc: "異常な高額報酬提示" }
        ],
        cat2: [
            { word: "受け子 募集", desc: "特殊詐欺の現金・カード回収係" },
            { word: "出し子 募集", desc: "ATMでの不正現金引き出し役" },
            { word: "口座買取", desc: "犯罪用インフラ（口座）の調達" },
            { word: "シム買取", desc: "飛ばし携帯・SIMカードの売買" },
            { word: "郵送案件", desc: "騙し取ったカード等の転送係" },
            { word: "書類回収", desc: "受け子をカモフラージュする表現" }
        ],
        cat3: [
            { word: "詳細はシグナル", desc: "暗号化アプリSignalへの誘導" },
            { word: "シグナル 移行", desc: "足跡を消すための定番文句" },
            { word: "テレグラム 案件", desc: " Telegramでの指示待ち要員募集" },
            { word: "裏に回れる方", desc: "秘匿連絡への移行が条件の募集" },
            { word: "Session 募集", desc: "ID不要の秘匿アプリへの誘導" }
        ],
        cat4: [
            { word: "🥦 手渡し", desc: "大麻密売（ブロッコリー絵文字）" },
            { word: "野菜 手押し", desc: "大麻の対面密売" },
            { word: "🍧 冷茶", desc: "覚醒剤密売（かき氷絵文字）" },
            { word: "アイス チャリ", desc: "覚醒剤の自転車配達" },
            { word: "🍯 リキッド", desc: "液体大麻（ハチミツ絵文字）" },
            { word: "💊 罰", desc: "MDMA・錠剤（バツ・ペケ）" },
            { word: "パケ 郵送", desc: "薬物のレターパック等での密輸" }
        ],
        cat5: [
            { word: "タスク副業", desc: "いいねや動画評価を騙る最新詐欺" },
            { word: "動画評価 副業", desc: "最初に数千円払い信用させる手口" },
            { word: "初心者ずるずる", desc: "TikTok等でのカモ探しワード" },
            { word: "放置ビジネス", desc: "不労所得をうたう投資詐欺誘導" },
            { word: "裏垢女子 副業", desc: "botを用いた悪質サイト誘導" }
        ]
    };

    // ページ読み込み時にボタンを動的生成
    window.onload = function() {
        for (let cat in keywordData) {
            const targetGrid = document.getElementById('grid-' + cat);
            keywordData[cat].forEach(item => {
                const card = document.createElement('div');
                card.className = 'keyword-card';
                
                // URLエンコード処理
                const encWord = encodeURIComponent(item.word);
                
                // 各SNSの検索URL（Xは最新順、Yay!は投稿検索に固定）
                const urlX = `https://x.com/search?q=${encWord}&f=live`;
                const urlInsta = `https://www.instagram.com/explore/tags/${encWord.replace(/%20/g, '')}/`;
                const urlTiktok = `https://www.tiktok.com/search?q=${encWord}`;
                const urlYay = `https://yay.space/search/posts?query=${encWord}`;

                card.innerHTML = `
                    <div class="keyword-header">
                        <span class="keyword-name">${item.word}</span>
                        <span class="keyword-desc">${item.desc}</span>
                    </div>
                    <div class="sns-grid">
                        <a href="${urlX}" target="_blank" class="sns-btn btn-x">X</a>
                        <a href="${urlInsta}" target="_blank" class="sns-btn btn-insta">Insta</a>
                        <a href="${urlTiktok}" target="_blank" class="sns-btn btn-tiktok">TikTok</a>
                        <a href="${urlYay}" target="_blank" class="sns-btn btn-yay">Yay!</a>
                    </div>
                `;
                targetGrid.appendChild(card);
            });
        }
    };

    // カテゴリ切り替えロジック
    function switchCategory(catId) {
        const containers = document.querySelectorAll('.category-container');
        containers.forEach(c => c.classList.remove('active'));

        const tabs = document.querySelectorAll('.tab-btn');
        tabs.forEach(t => t.classList.remove('active'));

        document.getElementById(catId).classList.add('active');
        event.currentTarget.classList.add('active');
    }
</script>

</body>
</html>
