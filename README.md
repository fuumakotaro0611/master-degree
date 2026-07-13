<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>修士課程・目標管理カウントダウン</title>
    <style>
        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            background-color: #f0f4f8;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .card {
            background: white;
            padding: 40px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            width: 100%;
            max-width: 600px;
        }
        .section {
            margin-bottom: 30px;
        }
        .section:last-child {
            margin-bottom: 0;
        }
        h2 {
            font-size: 1.1rem;
            color: #7f8c8d;
            margin: 0 0 10px 0;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            border-left: 4px solid #3498db;
            padding-left: 10px;
        }
        .vision-text {
            font-size: 1.3rem;
            font-weight: bold;
            color: #2c3e50;
            line-height: 1.5;
        }
        /* カウントダウンのスタイル */
        .countdown {
            display: flex;
            gap: 12px;
            margin-top: 10px;
        }
        .time-box {
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            padding: 12px;
            border-radius: 8px;
            flex: 1;
            text-align: center;
        }
        .number {
            font-size: 2rem;
            font-weight: bold;
            color: #1e293b;
            display: block;
        }
        .label {
            font-size: 0.75rem;
            color: #64748b;
        }
        /* 進捗バーのスタイル */
        .progress-container {
            background: #e2e8f0;
            border-radius: 10px;
            height: 12px;
            width: 100%;
            overflow: hidden;
            margin-top: 8px;
        }
        .progress-bar {
            background: linear-gradient(90deg, #3498db, #2ecc71);
            height: 100%;
            width: 0%;
            transition: width 0.5s ease;
        }
        .progress-text {
            font-size: 0.85rem;
            color: #64748b;
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
        }
        /* 期間目標リストのスタイル */
        .milestone-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .milestone-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px dashed #e2e8f0;
        }
        .milestone-item:last-child {
            border-bottom: none;
        }
        .milestone-date {
            font-size: 0.9rem;
            color: #7f8c8d;
            font-weight: 500;
        }
    </style>
</head>
<body>

<div class="card">
    <!-- 1. 目的・長期目標 -->
    <div class="section">
        <h2>修士課程の目的・大目標</h2>
        <div class="vision-text">
            「〇〇のメカニズムを解明し、国際学会で成果を発表して納得のいく形で学位を取得する！」
        </div>
    </div>

    <!-- 2. 卒業カウントダウンと全体の進捗率 -->
    <div class="section">
        <h2>修士卒業まで（全体の進捗）</h2>
        <div class="countdown">
            <div class="time-box"><span class="number" id="days">00</span><span class="label">日</span></div>
            <div class="time-box"><span class="number" id="hours">00</span><span class="label">時間</span></div>
            <div class="time-box"><span class="number" id="minutes">00</span><span class="label">分</span></div>
            <div class="time-box"><span class="number" id="seconds">00</span><span class="label">秒</span></div>
        </div>
        <div class="progress-container">
            <div class="progress-bar" id="progressBar"></div>
        </div>
        <div class="progress-text">
            <span>開始 (2026/04/01)</span>
            <span id="progressPercentage">0%</span>
            <span>卒業 (2028/03/25)</span>
        </div>
    </div>

    <!-- 3. 期間ごとのマイルストーン目標 -->
    <div class="section">
        <h2>期間・日時での目標（マイルストーン）</h2>
        <ul class="milestone-list">
            <li class="milestone-item">
                <span>中間発表・予備審査の突破</span>
                <span class="milestone-date">2027年 7月頃</span>
            </li>
            <li class="milestone-item">
                <span>主要データの測定・解析完了</span>
                <span class="milestone-date">2027年 10月末</span>
            </li>
            <li class="milestone-item">
                <span>修士論文の執筆完了・提出</span>
                <span class="milestone-date">2028年 1月中旬</span>
            </li>
        </ul>
    </div>
</div>

<script>
    // 【設定】期間の基準日と卒業予定日
    const startDate = new Date(2026, 3, 1, 0, 0, 0);   // 開始日 (2026年4月1日) ※月は-1
    const targetDate = new Date(2028, 2, 25, 10, 0, 0); // 卒業日 (2028年3月25日) ※月は-1

    function updateDashboard() {
        const now = new Date();
        const totalDuration = targetDate - startDate;
        const elapsedDuration = now - startDate;
        const remainingDuration = targetDate - now;

        // 1. カウントダウンの計算
        if (remainingDuration <= 0) {
            document.getElementById('days').innerText = '00';
            document.getElementById('hours').innerText = '00';
            document.getElementById('minutes').innerText = '00';
            document.getElementById('seconds').innerText = '00';
            document.getElementById('progressBar').style.width = '100%';
            document.getElementById('progressPercentage').innerText = '100%';
            clearInterval(timerInterval);
            return;
        }

        const days = Math.floor(remainingDuration / (1000 * 60 * 60 * 24));
        const hours = Math.floor((remainingDuration % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const minutes = Math.floor((remainingDuration % (1000 * 60 * 60)) / (1000 * 60));
        const seconds = Math.floor((remainingDuration % (1000 * 60)) / 1000);

        document.getElementById('days').innerText = String(days).padStart(2, '0');
        document.getElementById('hours').innerText = String(hours).padStart(2, '0');
        document.getElementById('minutes').innerText = String(minutes).padStart(2, '0');
        document.getElementById('seconds').innerText = String(seconds).padStart(2, '0');

        // 2. 進捗バーの計算
        let progressPercent = (elapsedDuration / totalDuration) * 100;
        progressPercent = Math.max(0, Math.min(100, progressPercent)); // 0%〜100%の間に収める
        
        document.getElementById('progressBar').style.width = progressPercent.toFixed(2) + '%';
        document.getElementById('progressPercentage').innerText = progressPercent.toFixed(1) + '%';
    }

    const timerInterval = setInterval(updateDashboard, 1000);
    updateDashboard();
</script>

</body>
</html># master-degree
