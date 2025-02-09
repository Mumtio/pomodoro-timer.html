<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Timer 🌸</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&family=Pacifico&display=swap');

        * {
            font-family: "Quicksand", sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(135deg, #f8d7e8, #f0e2ff);
        }

        .card {
            background: white;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0px 6px 15px rgba(0, 0, 0, 0.1);
            max-width: 350px;
            width: 90%;
            position: relative;
        }

        .washi-tape {
            width: 80px;
            height: 30px;
            background: url("https://i.imgur.com/bmGfBdM.png") no-repeat center/cover;
            position: absolute;
            top: -10px;
            left: 20px;
            transform: rotate(-10deg);
        }

        .title {
            font-size: 22px;
            font-weight: bold;
            color: #ff7fa3;
            font-family: "Pacifico", cursive;
            margin-bottom: 10px;
        }

        .settings {
            margin-top: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
        }

        .timer-container {
            position: relative;
            width: 180px;
            height: 180px;
            margin: 20px auto;
        }

        .circle {
            stroke-dasharray: 565.48;
            stroke-dashoffset: 565.48;
            transition: stroke-dashoffset 1s linear;
        }

        .water {
            width: 100%;
            height: 100%;
            background: linear-gradient(to top, #98d8f4, #d1f3ff);
            position: absolute;
            bottom: 0;
            border-radius: 50%;
            transition: height 1s linear;
        }

        .time {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 26px;
            font-weight: bold;
            color: #ff7fa3;
        }

        input {
            width: 60px;
            padding: 6px;
            border: none;
            border-radius: 8px;
            background: #ffd1dc;
            font-size: 16px;
            text-align: center;
        }

        .button-container {
            margin-top: 15px;
        }

        button {
            background: linear-gradient(135deg, #ffb6c1, #ff7fa3);
            border: none;
            color: white;
            padding: 12px 18px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            margin: 5px;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            box-shadow: 0px 4px 8px rgba(255, 105, 180, 0.3);
        }

        button:hover {
            transform: scale(1.1);
            box-shadow: 0px 6px 12px rgba(255, 105, 180, 0.5);
        }
    </style>
</head>
<body>

    <div class="card">
        <div class="washi-tape"></div>
        <div class="title">🌸 Pomodoro Timer 🌸</div>

        <div class="settings">
            ⏳ Time: <input type="number" id="customTime" value="25" min="1"> min
        </div>

        <div class="timer-container">
            <div class="water" id="water"></div>
            <div class="time" id="timer">25:00</div>
        </div>

        <div class="button-container">
            <button id="start">Start</button>
            <button id="reset">Reset</button>
        </div>
    </div>

    <script>
        let timerDisplay = document.getElementById("timer");
        let startButton = document.getElementById("start");
        let resetButton = document.getElementById("reset");
        let customTime = document.getElementById("customTime");
        let water = document.getElementById("water");
        let timeLeft = 25 * 60;
        let interval;
        let running = false;
        let totalLength = 2 * Math.PI * 100;

        function updateTimerDisplay() {
            let minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;
            timerDisplay.innerHTML = `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;

            let progress = (timeLeft / (parseInt(customTime.value) * 60)) * 100;
            water.style.height = `${100 - progress}%`; // Water fills up
        }

        function startTimer() {
            if (!running) {
                running = true;
                timeLeft = parseInt(customTime.value) * 60;
                interval = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(interval);
                        alert("🎉 Pomodoro complete! You did it! 🌟");
                        running = false;
                        water.style.height = "100%"; // Fully filled water
                    }
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(interval);
            timeLeft = parseInt(customTime.value) * 60;
            running = false;
            water.style.height = "0%";
            updateTimerDisplay();
        }

        customTime.addEventListener("change", () => {
            timeLeft = parseInt(customTime.value) * 60;
            updateTimerDisplay();
        });

        startButton.addEventListener("click", startTimer);
        resetButton.addEventListener("click", resetTimer);
        updateTimerDisplay();
    </script>

</body>
</html>
