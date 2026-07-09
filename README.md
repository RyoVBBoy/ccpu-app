<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCPU サイバーパトロール・通報支援ツール</title>
    <style>
        /* Pro Finderアプリを意識した、信頼感のあるアイラック・ブルーを基調としたカラー構成 */
        :root {
            --primary-color: #1A365D; /* ディープブルー（ヘッダー・メイン） */
            --secondary-color: #2B6CB0; /* 明るめのブルー（ボタン・アクティブ） */
            --bg-color: #F7FAFC; /* 薄いグレーの背景 */
            --card-bg: #FFFFFF; /* 純白のカード */
            --text-color: #2D3748; /* ダークグレーのテキスト */
            --accent-color: #E53E3E; /* 警告・通報を意識した赤 */
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
            line-height: 1.6;
            padding-bottom: 40px;
        }

        header {
            background-color: var(--primary-color);
            color: white;
            text-align: center;
            padding: 25px 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        header h1 {
            font-size: 1.8rem;
            margin-bottom: 5px;
        }

        header p {
            font-size: 0.9rem;
            opacity: 0.9;
        }

        .container {
            max-width: 800px;
            margin: 30px auto;
            padding: 0 15px;
        }

        .intro-box {
            background-color: var(--card-bg);
            border-left: 5px solid var(--secondary-color);
            padding: 15px;
            margin-bottom: 25px;
            border-radius: 4px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        /* SNS切り替えタブ */
        .tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            overflow-x: auto;
            padding-bottom: 5px;
        }

        .tab-btn {
            flex: 1;
            min-width: 100px;
            padding: 12px;
            background-color: #E2E8F0;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            color: var(--primary-color);
            transition: all 0.2s ease;
            text-align: center;
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

        .sns-title {
            font-size: 1.3rem;
            color: var(--primary-color);
            margin-bottom: 15px;
            border-bottom: 2px solid #E2E8F0;
            padding-bottom: 8px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 12px;
        }

        /* 検索ジャンプボタン */
        .search-btn {
            display: block;
            padding: 14px 10px;
            background-color: #EDF2F7;
            color: var(--text-color);
            text-decoration: none;
            border-radius: 6px;
            text-align: center;
            font-weight: 500;
            font-size: 0.95rem;
            border: 1px solid #CBD5E0;
            transition: all 0.2s ease;
            box-shadow: 0 2px 4px rgba(0,0,0,0.02);
        }

        .search-btn:hover {
            background-color: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
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
    <p>SNS別・通報対象キーワード一括検索ツール</p>
</header>

<div class="container">
    <div class="intro-box">
        <p>🚨 <strong>使用方法:</strong> 下記のSNSタブを選択し、キーワードボタンをタップすると、そのSNSの検索画面（キーワード入力済み）に直接ジャンプします。違法・有害情報の早期発見と通報に活用してください。</p>
    </div>

    <div class="tabs">
        <button class="tab-btn active" onclick="switchSNS('x')">X (Twitter)</button>
        <button class="tab-btn" onclick="switchSNS('instagram')">Instagram</button>
        <button class="tab-btn" onclick="switchSNS('tiktok')">TikTok</button>
    </div>

    <div id="x" class="keyword-container active">
        <h2 class="sns-title">X (旧Twitter) 通報対象キーワード</h2>
        <div class="grid">
            <a href="https://x.com/search?q=%23%E9%AB%98%E9%A1%8D%E6%A1%88%E4%BB%B6&f=live" target="_blank" class="search-btn">#高額案件</a>
            <a href="https://x.com/search?q=%23%E5%8D%B3%E6%97%A5%E5%8D%B3%E9%87%91&f=live" target="_blank" class="search-btn">#即日即金</a>
            <a href="https://x.com/search?q=%E9%97%87%E3%83%90%E3%82%A4%E3%83%88&f=live" target="_blank" class="search-btn">闇バイト</a>
            <a href="https://x.com/search?q=%E9%81%8B%E6%90%AC%E5%BD%B9&f=live" target="_blank" class="search-btn">運搬役</a>
            <a href="https://x.com/search?q=%E5%8F%A3%E5%BA%A7%E8%B2%B7%E5%8F%96&f=live" target="_blank" class="search-btn">口座買取</a>
            <a href="https://x.com/search?q=%E5%89%AF%E6%A5%AD%20%E5%8B%A2%E5%8A%9B&f=live" target="_blank" class="search-btn">副業 勢力</a>
            <a href="https://x.com/search?q=%E8%A3%8F%E5%9E%A2%E5%A5%B3%E5%AD%90%20%E5%89%AF%E6%A5%AD&f=live" target="_blank" class="search-btn">裏垢女子 副業</a>
            <a href="https://x.com/search?q=%E3%82%BF%E3%82%B9%E3%82%AF%E5%89%AF%E6%A5%AD&f=live" target="_blank" class="search-btn">タスク副業</a>
        </div>
    </div>

    <div id="instagram" class="keyword-container">
        <h2 class="sns-title">Instagram 通報対象キーワード</h2>
        <div class="grid">
            <a href="https://www.instagram.com/explore/tags/高額案件/" target="_blank" class="search-btn">#高額案件</a>
            <a href="https://www.instagram.com/explore/tags/副業初心者/" target="_blank" class="search-btn">#副業初心者</a>
            <a href="https://www.instagram.com/explore/tags/即日即金/" target="_blank" class="search-btn">#即日即金</a>
            <a href="https://www.instagram.com/explore/tags/放置ビジネス/" target="_blank" class="search-btn">#放置ビジネス</a>
            <a href="https://www.instagram.com/explore/tags/隙間時間で稼げる/" target="_blank" class="search-btn">#隙間時間で稼げる</a>
        </div>
    </div>

    <div id="tiktok" class="keyword-container">
        <h2 class="sns-title">TikTok 通報対象キーワード</h2>
        <div class="grid">
            <a href="https://www.tiktok.com/search?q=%E9%97%87%E3%83%90%E3%82%A4%E3%83%88" target="_blank" class="search-btn">闇バイト</a>
            <a href="https://www.tiktok.com/search?q=%E9%AB%98%E9%A1%8D%E6%A1%88%E4%BB%B6" target="_blank" class="search-btn">高額案件</a>
            <a href="https://www.tiktok.com/search?q=%E5%88%9D%E5%BF%83%E8%80%85%E3%81%A5%E3%82%8B%E3%81%9A%E3%82%8B" target="_blank" class="search-btn">初心者ずるずる</a>
            <a href="https://www.tiktok.com/search?q=%E5%89%AF%E6%A5%AD%E3%82%B9%E3%83%9E%E3%83%9B" target="_blank" class="search-btn">副業スマホ</a>
        </div>
    </div>
</div>

<footer>
    <p>© 2026 CCPU Cyber Crime Prevention Unit. All Rights Reserved.</p>
</footer>

<script>
    function switchSNS(snsId) {
        // すべてのコンテンツを非表示にする
        const containers = document.querySelectorAll('.keyword-container');
        containers.forEach(container => container.classList.remove('active'));

        // すべてのタブの活性化を解除
        const tabs = document.querySelectorAll('.tab-btn');
        tabs.forEach(tab => tab.classList.remove('active'));

        // 選択されたSNSを表示
        document.getElementById(snsId).classList.add('active');
        
        // クリックされたタブを活性化
        event.currentTarget.classList.add('active');
    }
</script>

</body>
</html># ccpu-app
