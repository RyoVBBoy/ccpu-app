<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CCPU サイバーパトロール・インシデントフィード</title>
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

        /* 📱 フィードコンテナ */
        .timeline {
            max-width: 650px;
            margin: 0 auto;
            padding: 15px;
        }

        /* ✉️ インシデント報告ログカード */
        .post-card {
            background-color: var(--bg-card);
            border-radius: 8px;
            padding: 16px;
            margin-bottom: 15px;
            border: 1px solid #1E293B;
            position: relative;
            transition: border-color 0.3s;
        }
        .post-card:hover {
            border-color: var(--border-neon);
            box-shadow: 0 0 10px var(--accent-glow);
        }

        /* アナリスト情報（パトロール隊員） */
        .post-user-info {
            display: flex;
            align-items: center;
            margin-bottom: 12px;
        }

        .avatar {
            width: 38px;
            height: 38px;
            background-color: #1E293B;
            border: 1px solid var(--border-neon);
            border-radius: 4px;
            margin-right: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #38BDF8;
            font-size: 0.8rem;
        }

        .user-meta {
            display: flex;
            flex-direction: column;
        }

        .user-name {
            font-size: 0.9rem;
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
            padding: 4px 10px;
            border-radius: 4px;
            font-size: 0.75rem;
            font-weight: bold;
            letter-spacing: 1px;
        }
        .bg-critical { background-color: rgba(239, 68, 68, 0.2); border: 1px solid var(--signal-critical); color: #FCA5A5; }
        .bg-warning { background-color: rgba(245, 158, 11, 0.2); border: 1px solid var(--signal-warning); color: #FDE68A; }

        /* ログ本文 */
        .post-content {
            font-size: 0.9rem;
            white-space: pre-wrap;
            margin-bottom: 12px;
            word-break: break-all;
            color: #CBD5E1;
        }

        .post-link {
            display: inline-block;
            color: #38BDF8;
            text-decoration: none;
            font-size: 0.8rem;
            margin-top: 5px;
            word-break: break-all;
            border-bottom: 1px dashed #38BDF8;
        }
        .post-link:hover {
            color: #7DD3FC;
        }

        /* 📊 コマンド・アクションバー */
        .action-bar {
            display: flex;
            border-top: 1px solid #1E293B;
            padding-top: 10px;
            color: var(--text-sub);
            font-size: 0.8rem;
            gap: 24px;
        }

        .action-item {
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
            transition: color 0.2s;
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
            border-radius: 8px;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 0 15px rgba(14, 165, 233, 0.4);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 200;
            transition: all 0.2s;
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
            border-radius: 12px;
            padding: 24px;
            box-shadow: 0 0 25px rgba(14, 165, 233, 0.3);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            font-weight: bold;
            color: #38BDF8;
            letter-spacing: 1px;
        }

        .close-btn { cursor: pointer; color: var(--text-sub); }
        .close-btn:hover { color: var(--signal-critical); }

        .form-group {
            margin-bottom: 16px;
        }
        .form-group label {
            display: block;
            font-size: 0.75rem;
            font-weight: bold;
            margin-bottom: 6px;
            color: var(--text-sub);
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .form-group select, .form-group textarea, .form-group input {
            width: 100%;
            padding: 10px;
            background-color: #1E293B;
            border: 1px solid #334155;
            border-radius: 6px;
            font-size: 0.9rem;
            color: white;
        }
        .form-group select:focus, .form-group textarea:focus, .form-group input:focus {
            outline: none;
            border-color: var(--border-neon);
        }

        .submit-btn {
            width: 100%;
            padding: 12px;
            background-color: var(--border-neon);
            color: #020617;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 10px;
            letter-spacing: 1px;
            transition: background-color 0.2s;
        }
        .submit-btn:hover {
            background-color: #7DD3FC;
        }
    </style>
</head>
<body>

<header>
    <h1>CCPU // CYBER PATROL INCIDENT HUB</h1>
    <div class="header-status">SYSTEM ONLINE</div>
</header>

<div class="timeline" id="timeline">
    
    <div class="post-card">
        <span class="judge-badge bg-critical">CRITICAL [即通報]</span>
        <div class="post-user-info">
            <div class="avatar">AN-A</div>
            <div class="user-meta">
                <span class="user-name">パトロールアナリスト Alpha</span>
                <span class="post-time">今日 13:15 · 監視対象プラットフォームにて検知</span>
            </div>
        </div>
        <div class="post-content">タイムライン上で「即日5万、詳細はシグナルへ。足跡つかないホワイト」という定型勧誘を検知。アカウント作成から48時間以内。完全に実行役（受け子）の募集。</div>
        <a href="#" target="_blank" class="post-link">🔗 該当ログの証拠ソースURLを開く</a>
        <div class="action-bar">
            <div class="action-item" onclick="alert('カウンターシグナル（通報カウンター）を同期しました')">🚨 通報連携完了</div>
            <div class="action-item">💬 解析ログコメント (0)</div>
        </div>
    </div>

    <div class="post-card">
        <span class="judge-badge bg-warning">WARNING [要警戒]</span>
        <div class="post-user-info">
            <div class="avatar">AN-B</div>
            <div class="user-meta">
                <span class="user-name">パトロールアナリスト Beta</span>
                <span class="post-time">今日 12:40 · 公開フィードにて検知</span>
            </div>
        </div>
        <div class="post-content">プロフィール欄が「#高額案件」のみで、直近の投稿履歴が一切ない新規不審アカウントが、特定地域の学生層アカウントを大量フォローしている事象を確認。DM勧誘の前兆の可能性大。</div>
        <div class="action-bar">
            <div class="action-item">🚨 通報連携完了</div>
            <div class="action-item">💬 解析ログコメント (2)</div>
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
                    <option value="匿名チャットプラットフォーム">匿名チャットプラットフォーム</option>
                    <option value="主要SNS (X等)">主要SNS (X等)</option>
                    <option value="画像・動画配信系">画像・動画配信系</option>
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
                <textarea id="postText" rows="4" placeholder="検知した書き込み内容、使用されている隠語、アカウントの挙動などを入力" required></textarea>
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
