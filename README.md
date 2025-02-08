<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Timer</title>
    <style>
        * {
            font-family: "Poppins", sans-serif;
            text-align: center;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #ffe4e1; /* Soft pink */
        }

        h1 {
            color: #ff69b4; /* Cute pink */
            font-size: 26px;
        }

        .timer-container {
            position: relative;
            width: 200px;
            height: 200px;
        }

        .circle {
            stroke-dasharray: 565.48;
            stroke-dashoffset: 0;
            transition: stroke-dashoffset 1s linear;
        }

        .time {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 24px;
            color: #ff69b4;
        }

        button {
            background-color: #ff69b4;
            border: none;
            color: white;
            padding: 10px 15px;
            font-size: 16px;
            border-radius: 10px;
            cursor: pointer;
            margin-top: 15px;
        }

        button:hover {
            background-color: #ff1493;
        }
    </style>
</head>
<body>

    <h1>🎀 Pomodoro Timer 🎀</h1>
    <div class="timer-container">
        <svg width="200" height="200">
            <circle cx="100" cy="100" r="90" stroke="#ff69b4" stroke-width="10" fill="none" class="circle"/>
        </svg>
        <div class="time" id="timer">25:00</div>
    </div>

    <button id="start">Start</button>
    <button id="reset">Reset</button>

    <script>
        let timerDisplay = document.getElementById("timer");
        let startButton = document.getElementById("start");
        let resetButton = document.getElementById("reset");
        let timeLeft = 25 * 60; // Default 25 minutes
        let interval;
        let running = false;
        let circle = document.querySelector(".circle");
        let totalLength = 2 * Math.PI * 90; // Circumference of circle

        function updateTimerDisplay() {
            let minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;
            timerDisplay.innerHTML = `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;

            // Animate the countdown circle
            let progress = (timeLeft / (25 * 60)) * totalLength;
            circle.style.strokeDashoffset = totalLength - progress;
        }

        function startTimer() {
            if (!running) {
                running = true;
                interval = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(interval);
                        alert("🍓 Pomodoro session complete!");
                        running = false;
                    }
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(interval);
            timeLeft = 25 * 60;
            running = false;
            updateTimerDisplay();
        }

        startButton.addEventListener("click", startTimer);
        resetButton.addEventListener("click", resetTimer);

        updateTimerDisplay();
    </script>

</body>
</html>
