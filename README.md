<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Professional Stopwatch</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: url("https://i.pinimg.com/564x/4e/37/75/4e377556c109eb51b601be30dfa69a98.jpg");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }



        .stopwatch-container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            padding: 40px;
            text-align: center;
            max-width: 500px;
            width: 100%;
        }

        .title {
            color: #333;
            margin-bottom: 30px;
            font-size: 2.5em;
            font-weight: 300;
            letter-spacing: 2px;
        }

        .time-display {
            font-size: 4em;
            font-weight: 200;
            color: #2c3e50;
            margin-bottom: 40px;
            font-family: 'Courier New', monospace;
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            border: 3px solid #e9ecef;
            letter-spacing: 3px;
            box-shadow: inset 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .controls {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 50px;
            font-size: 1.1em;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
            min-width: 120px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn-start {
            background: linear-gradient(45deg, #27ae60, #2ecc71);
            color: white;
        }

        .btn-pause {
            background: linear-gradient(45deg, #f39c12, #e67e22);
            color: white;
        }

        .btn-reset {
            background: linear-gradient(45deg, #e74c3c, #c0392b);
            color: white;
        }

        .btn-lap {
            background: linear-gradient(45deg, #3498db, #2980b9);
            color: white;
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .btn:disabled:hover {
            transform: none;
            box-shadow: none;
        }

        .lap-times {
            max-height: 300px;
            overflow-y: auto;
            margin-top: 20px;
        }

        .lap-times h3 {
            color: #2c3e50;
            margin-bottom: 15px;
            font-weight: 400;
        }

        .lap-item {
            background: #f8f9fa;
            margin: 8px 0;
            padding: 12px 20px;
            border-radius: 10px;
            border-left: 4px solid #3498db;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all 0.2s ease;
        }

        .lap-item:hover {
            background: #e9ecef;
            transform: translateX(5px);
        }

        .lap-number {
            font-weight: 600;
            color: #3498db;
        }

        .lap-time {
            font-family: 'Courier New', monospace;
            color: #2c3e50;
            font-size: 1.1em;
        }

        .fastest {
            border-left-color: #27ae60;
            background: linear-gradient(90deg, #d5f4e6, #f8f9fa);
        }

        .slowest {
            border-left-color: #e74c3c;
            background: linear-gradient(90deg, #fce4e1, #f8f9fa);
        }

        .fastest .lap-number {
            color: #27ae60;
        }

        .slowest .lap-number {
            color: #e74c3c;
        }

        .lap-times::-webkit-scrollbar {
            width: 8px;
        }

        .lap-times::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }

        .lap-times::-webkit-scrollbar-thumb {
            background: #3498db;
            border-radius: 10px;
        }

        .stats {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 2px solid #ecf0f1;
        }

        .stat {
            text-align: center;
        }

        .stat-value {
            font-size: 1.2em;
            font-weight: 600;
            color: #2c3e50;
            font-family: 'Courier New', monospace;
        }

        .stat-label {
            font-size: 0.9em;
            color: #7f8c8d;
            margin-top: 5px;
        }

        @media (max-width: 600px) {
            .stopwatch-container {
                padding: 30px 20px;
            }

            .title {
                font-size: 2em;
            }

            .time-display {
                font-size: 2.5em;
            }

            .controls {
                flex-direction: column;
                align-items: center;
            }

            .btn {
                width: 100%;
                max-width: 250px;
            }
        }
    </style>
</head>

<body>
    <div class="stopwatch-container">
        <h1 class="title">STOPWATCH</h1>

        <div class="time-display" id="timeDisplay">00:00:00</div>

        <div class="controls">
            <button class="btn btn-start" id="startBtn" onclick="startStopwatch()">Start</button>
            <button class="btn btn-lap" id="lapBtn" onclick="recordLap()" disabled>Lap</button>
            <button class="btn btn-reset" id="resetBtn" onclick="resetStopwatch()">Reset</button>
        </div>

        <div class="lap-times" id="lapTimes"></div>

        <div class="stats" id="stats" style="display: none;">
            <div class="stat">
                <div class="stat-value" id="fastestTime">--:--:--</div>
                <div class="stat-label">Fastest</div>
            </div>
            <div class="stat">
                <div class="stat-value" id="slowestTime">--:--:--</div>
                <div class="stat-label">Slowest</div>
            </div>
            <div class="stat">
                <div class="stat-value" id="avgTime">--:--:--</div>
                <div class="stat-label">Average</div>
            </div>
        </div>
    </div>

    <script>
        let startTime = 0;
        let elapsedTime = 0;
        let timerInterval = null;
        let isRunning = false;
        let lapCount = 0;
        let lapTimes = [];
        let lastLapTime = 0;

        const timeDisplay = document.getElementById('timeDisplay');
        const startBtn = document.getElementById('startBtn');
        const lapBtn = document.getElementById('lapBtn');
        const resetBtn = document.getElementById('resetBtn');
        const lapTimesContainer = document.getElementById('lapTimes');
        const statsContainer = document.getElementById('stats');

        function formatTime(milliseconds) {
            const totalSeconds = Math.floor(milliseconds / 1000);
            const minutes = Math.floor(totalSeconds / 60);
            const seconds = totalSeconds % 60;
            const ms = Math.floor((milliseconds % 1000) / 10);

            return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}:${ms.toString().padStart(2, '0')}`;
        }

        function updateDisplay() {
            const currentTime = isRunning ? Date.now() - startTime + elapsedTime : elapsedTime;
            timeDisplay.textContent = formatTime(currentTime);
        }

        function startStopwatch() {
            if (!isRunning) {
                startTime = Date.now();
                isRunning = true;
                timerInterval = setInterval(updateDisplay, 10);

                startBtn.textContent = 'Pause';
                startBtn.className = 'btn btn-pause';
                lapBtn.disabled = false;
            } else {
                elapsedTime += Date.now() - startTime;
                isRunning = false;
                clearInterval(timerInterval);

                startBtn.textContent = 'Start';
                startBtn.className = 'btn btn-start';
                lapBtn.disabled = true;
            }
        }

        function resetStopwatch() {
            clearInterval(timerInterval);
            startTime = 0;
            elapsedTime = 0;
            isRunning = false;
            lapCount = 0;
            lapTimes = [];
            lastLapTime = 0;

            timeDisplay.textContent = '00:00:00';
            startBtn.textContent = 'Start';
            startBtn.className = 'btn btn-start';
            lapBtn.disabled = true;

            lapTimesContainer.innerHTML = '';
            statsContainer.style.display = 'none';
        }

        function recordLap() {
            if (!isRunning) return;

            const currentTime = Date.now() - startTime + elapsedTime;
            const lapTime = currentTime - lastLapTime;
            lapCount++;

            lapTimes.push(lapTime);
            lastLapTime = currentTime;

            displayLaps();
            updateStats();
        }

        function displayLaps() {
            const lapContainer = document.createElement('div');
            lapContainer.innerHTML = `
                <h3>Lap Times</h3>
                ${lapTimes.map((time, index) => `
                    <div class="lap-item" id="lap-${index}">
                        <span class="lap-number">Lap ${index + 1}</span>
                        <span class="lap-time">${formatTime(time)}</span>
                    </div>
                `).reverse().join('')}
            `;

            lapTimesContainer.innerHTML = lapContainer.innerHTML;
            highlightFastestSlowest();
        }

        function highlightFastestSlowest() {
            if (lapTimes.length < 2) return;

            const minTime = Math.min(...lapTimes);
            const maxTime = Math.max(...lapTimes);

            lapTimes.forEach((time, index) => {
                const lapElement = document.getElementById(`lap-${index}`);
                if (lapElement) {
                    lapElement.classList.remove('fastest', 'slowest');
                    if (time === minTime) {
                        lapElement.classList.add('fastest');
                    } else if (time === maxTime) {
                        lapElement.classList.add('slowest');
                    }
                }
            });
        }

        function updateStats() {
            if (lapTimes.length === 0) {
                statsContainer.style.display = 'none';
                return;
            }

            const fastest = Math.min(...lapTimes);
            const slowest = Math.max(...lapTimes);
            const average = lapTimes.reduce((sum, time) => sum + time, 0) / lapTimes.length;

            document.getElementById('fastestTime').textContent = formatTime(fastest);
            document.getElementById('slowestTime').textContent = formatTime(slowest);
            document.getElementById('avgTime').textContent = formatTime(average);

            statsContainer.style.display = 'flex';
        }

        document.addEventListener('keydown', (e) => {
            switch (e.code) {
                case 'Space':
                    e.preventDefault();
                    startStopwatch();
                    break;
                case 'KeyL':
                    if (isRunning) recordLap();
                    break;
                case 'KeyR':
                    resetStopwatch();
                    break;
            }
        });

        updateDisplay();
    </script>
</body>

</html>
