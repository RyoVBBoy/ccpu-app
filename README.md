<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCPU パトロール検索ハブ</title>
    <style>
        :root {
            --primary-color: #1A365D; /* ディープブルー（Pro Finderベース） */
            --secondary-color: #2B6CB0; /* アクティブブルー */
            --bg-color: #F7FAFC;
            --card-bg: #FFFFFF;
            --text-color: #2D3748;
        }

        * {
            box-shadow: border-box;
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            line-height: 1.6;
            padding-bottom: 50px;
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
        }

        header p {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .container {
            max-width: 900px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .intro-box {
            background-color: var(--card-bg);
            border-left: 5px solid #E53E3E; /* 警告の赤 */
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 4px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            font-size: 0.9rem;
        }

        /* SNS切り替えタブ */
        .tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .tab-btn {
            flex: 1;
            padding: 14px;
            background-color: #E2E8F0;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            color: var(--primary-color);
            transition: all 0.2s ease;
            text-align: center;
            font-size: 0.95rem;
        }

        .tab-btn.active {
            background-color: var(--secondary-color);
            color: white;
        }

        /* キーワードセクション */
        .keyword-container {
            display: none;
            background-color: var(--card-bg);
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .keyword-container.active {
            display: block;
        }

        .category-title {
            font-size: 1rem;
            color: var(--primary-color);
            margin: 20px 0 10px 0;
            padding-left: 8px;
            border-left: 3px solid var(--secondary-color);
            font-weight: bold;
        }

        .category-title:first-of-type {
            margin-top: 0;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 10px;
            margin-bottom: 15px;
        }

        /* 検索ジャンプボタン */
        .search-btn {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 12px 8px;
            background-color: #EDF2F7;
            color: var(--text-color);
            text-decoration: none;
            border-radius: 6px;
            text-align: center;
            font-weight: 500;
            font-size: 0.9rem;
            border: 1px solid #CBD5E0;
            transition: all 0.15s ease;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .search-btn:hover {
            background-color: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
            transform: translateY(-2px);
        }

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
    <p>最新隠語・絵文字対応 / 違法・有害投稿一括検索</p>
</header>

<div class="container">
    <div class="intro-box">
        📌 <strong>CCPUパトロールの注意点:</strong> ボタンを押すと各SNSの検索結果（Xは「最新順」）へ飛びます。隠語や絵文字（野菜、冷茶、アイス、叩き等）を用いた有害投稿、またはシグナル等への誘導文句を見つけたら、速やかに通報・スクショ保存を行ってください。
    </div>

    <!-- SNS切り替えタブ -->
    <div class="tabs">
        <button class="tab-btn active" onclick="switchSNS('x')">X (Twitter)</button>
        <button class="tab-btn" onclick="switchSNS('instagram')">Instagram</button>
        <button class="tab-btn" onclick="switchSNS('tiktok')">TikTok</button>
    </div>

    <!-- ==================== X (Twitter) ==================== -->
    <div id="x" class="keyword-container active">
        <div class="category-title">🚨 闇バイト・強盗・実行役募集（最新隠語）</div>
        <div class="grid">
            <a href="https://x.com/search?q=%E5%8F%A9%E3%81%8D%20%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">叩き 案件</a>
            <a href="https://x.com/search?q=%E3%82%BF%E3%82%B3%20%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">タコ 案件</a>
            <a href="https://x.com/search?q=%E6%91%BA%E8%88%AA%20%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">臨航 案件</a>
            <a href="https://x.com/search?q=%E6%91%BA%E5%8B%99%20%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">臨務 案件</a>
            <a href="https://x.com/search?q=%E9%81%8B%E6%90%AC%20%E4%BB%A3%E8%A1%8C&f=live" target="_blank" class="search-btn">運搬 代行</a>
            <a href="https://x.com/search?q=%E6%97%A5%E5%BD%A310%E4%B8%87&f=live" target="_blank" class="search-btn">日当10万</a>
        </div>

        <div class="category-title">💬 秘匿性の高いアプリへの誘導</div>
        <div class="grid">
            <a href="https://x.com/search?q=%E8%A9%B3%E7%B4%B0%E3%81%AF%E3%82%BV%E3%82%B0%E3%83%8A%E3%83%AB&f=live" target="_blank" class="search-btn">詳細はシグナル</a>
            <a href="https://x.com/search?q=Signal%20%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">Signal 案件</a>
            <a href="https://x.com/search?q=Telegram%20%E5%89%AF%E6%A5%AD&f=live" target="_blank" class="search-btn">Telegram 副業</a>
            <a href="https://x.com/search?q=%E3%83%86%E3%83%AC%E3%82%B0%E3%83%A9%E3%83%A0%20%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">テレグラム 案件</a>
        </div>

        <div class="category-title">🥦 薬物密売・隠語（絵文字含む）</div>
        <div class="grid">
            <a href="https://x.com/search?q=%E3%83%81%E3%83%A3%E3%83%AA%E6%89%8B%E6%B8%A1%E3%81%97&f=live" target="_blank" class="search-btn">チャリ手渡し</a>
            <a href="https://x.com/search?q=%E9%83%B5%E9%80%81%20%E5%AE%A4%E5%86%85%E3%82%B5%E3%83%AF%E3%83%BC&f=live" target="_blank" class="search-btn">郵送 室内サワー</a>
            <a href="https://x.com/search?q=%F0%9F%A5%A6%20%E6%89%8B%E6%B8%A1%E3%81%97&f=live" target="_blank" class="search-btn">🥦 手渡し</a>
            <a href="https://x.com/search?q=%F0%9F%8D%A5%20%E5%86%B7%E8%8C%B6&f=live" target="_blank" class="search-btn">🍧 冷茶</a>
            <a href="https://x.com/search?q=%E3%82%A2%E3%82%A4%E3%82%B9%20%E3%83%81%E3%83%A3%E3%83%AA&f=live" target="_blank" class="search-btn">アイス チャリ</a>
            <a href="https://x.com/search?q=%E9%83%B5%E9%80%81%20%E3%83%91%E3%82%B1&f=live" target="_blank" class="search-btn">郵送 パケ</a>
        </div>

        <div class="category-title">💳 特殊詐欺・出し子・口座売買</div>
        <div class="grid">
            <a href="https://x.com/search?q=%E5%87%BA%E3%81%97%E5%AD%90%20%E5%8B%9F%E9%9B%86&f=live" target="_blank" class="search-btn">出し子 募集</a>
            <a href="https://x.com/search?q=%E5%8F%A3%E5%BA%A7%E8%B2%B7%E5%8F%96%20%E5%8D%B3%E6%97%A5&f=live" target="_blank" class="search-btn">口座買取 即日</a>
            <a href="https://x.com/search?q=%E6%A1%88%E4%BB%B6%20%E5%86%85%E8%81%B7%20%E5%8F%A3%E5%BA%A7&f=live" target="_blank" class="search-btn">案件 内職 口座</a>
            <a href="https://x.com/search?q=%E3%82%B7%E3%83%A0%E5%9B%9E%E7%B7%9A%20%E8%B2%B7%E5%8F%96&f=live" target="_blank" class="search-btn">シム回線 買取</a>
        </div>
    </div>

    <!-- ==================== Instagram ==================== -->
    <div id="instagram" class="keyword-container">
        <div class="category-title">📸 ハッシュタグベースの副業・闇バイト勧誘</div>
        <div class="grid">
            <a href="https://www.instagram.com/explore/tags/高額案件/" target="_blank" class="search-btn">#高額案件</a>
            <a href="https://www.instagram.com/explore/tags/即日即金/" target="_blank" class="search-btn">#即日即金</a>
            <a href="https://www.instagram.com/explore/tags/裏案件/" target="_blank" class="search-btn">#裏案件</a>
            <a href="https://www.instagram.com/explore/tags/ホワイト案件/" target="_blank" class="search-btn">#ホワイト案件</a>
            <a href="https://www.instagram.com/explore/tags/叩き案件/" target="_blank" class="search-btn">#叩き案件</a>
            <a href="https://www.instagram.com/explore/tags/シグナル移行/" target="_blank" class="search-btn">#シグナル移行</a>
        </div>
        <div class="category-title">🌿 ストーリーズ等での薬物隠語ハッシュタグ</div>
        <div class="grid">
            <a href="https://www.instagram.com/explore/tags/野菜手渡し/" target="_blank" class="search-btn">#野菜手渡し</a>
            <a href="https://www.instagram.com/explore/tags/アイス手押し/" target="_blank" class="search-btn">#アイス手押し</a>
            <a href="https://www.instagram.com/explore/tags/手押し手渡し/" target="_blank" class="search-btn">#手押し手渡し</a>
        </div>
    </div>

    <!-- ==================== TikTok ==================== -->
    <div id="tiktok" class="keyword-container">
        <div class="category-title">🎵 動画・文字テロップによる勧誘（検索語句）</div>
        <div class="grid">
            <a href="https://www.tiktok.com/search?q=%E9%97%87%E3%83%90%E3%82%A4%E3%83%88%20%E6%9C%AC%E5%BD%93%E3%81%AE%E3%81%A8%E3%81%93%E3%82%8D" target="_blank" class="search-btn">闇バイト 本当のところ</a>
            <a href="https://www.tiktok.com/search?q=%E9%AB%98%E9%A1%8D%E6%A1%88%E4%BB%B6%20%E5%88%9D%E5%BF%83%E8%80%85" target="_blank" class="search-btn">高額案件 初心者</a>
            <a href="https://www.tiktok.com/search?q=%E3%81%9F%E3%81%9F%E3%81%8D%E6%A1%88%E4%BB%B6" target="_blank" class="search-btn">たたき案件</a>
            <a href="https://www.tiktok.com/search?q=%E3%82%B7%E3%83%B3%E3%82%B0%E3%83%AB%E3%83%9E%E3%83%B3%20%E5%89%AF%E6%A5%AD" target="_blank" class="search-btn">シングルマン 副業</a>
            <a href="https://www.tiktok.com/search?q=%E3%82%B7%E3%83%B0%E3%83%8A%E3%83%AB%20%E5%88%B0%E9%81%94" target="_blank" class="search-btn">シグナル 到達</a>
        </div>
    </div>
</div>

<footer>
    <p>© 2026 CCPU Cyber Crime Prevention Unit. All Rights Reserved.</p>
</footer>

<script>
    function switchSNS(snsId) {
        const containers = document.querySelectorAll('.keyword-container');
        containers.forEach(container => container.classList.remove('active'));

        const tabs = document.querySelectorAll('.tab-btn');
        tabs.forEach(tab => tab.classList.remove('active'));

        document.getElementById(snsId).classList.add('active');
        event.currentTarget.classList.add('active');
    }
</script>

</body>
</html>
