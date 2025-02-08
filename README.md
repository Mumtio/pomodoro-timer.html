<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Forest 🌸</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&family=Fredoka+One&display=swap');

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
            background: linear-gradient(135deg, #ffdde1, #ee9ca7);
        }

        h1 {
            color: #ff69b4;
            font-size: 28px;
            font-family: "Fredoka One", cursive;
            padding: 10px 20px;
            border-radius: 20px;
            background: white;
            box-shadow: 0px 4px 10px rgba(255, 105, 180, 0.3);
        }

        .timer-container {
            position: relative;
            width: 220px;
            height: 220px;
            margin-top: 20px;
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
            font-size: 26px;
            font-weight: bold;
            color: #ff69b4;
        }

        #tree {
            width: 120px;
            margin-top: 20px;
            transition: opacity 1s ease-in-out;
            filter: drop-shadow(0px 6px 10px rgba(0, 0, 0, 0.2));
        }

        .button-container {
            margin-top: 15px;
        }

        button {
            background: linear-gradient(135deg, #ff85a2, #ff4081);
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

    <h1>🎀 Pomodoro Forest 🎀</h1>
    <div class="timer-container">
        <svg width="220" height="220">
            <circle cx="110" cy="110" r="100" stroke="#ff69b4" stroke-width="10" fill="none" class="circle"/>
        </svg>
        <div class="time" id="timer">25:00</div>
    </div>

    <img id="tree" src="https://upload.wikimedia.org/wikipedia/commons/6/6b/Tree_animated.gif" alt="Tree" style="opacity: 0.2;">
    
    <div class="button-container">
        <button id="start">🌱 Start</button>
        <button id="reset">💔 Reset</button>
    </div>

    <script>
        let timerDisplay = document.getElementById("timer");
        let startButton = document.getElementById("start");
        let resetButton = document.getElementById("reset");
        let tree = document.getElementById("tree");
        let timeLeft = 25 * 60;
        let interval;
        let running = false;
        let totalLength = 2 * Math.PI * 100;
        let circle = document.querySelector(".circle");

        function updateTimerDisplay() {
            let minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;
            timerDisplay.innerHTML = `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
            let progress = (timeLeft / (25 * 60)) * totalLength;
            circle.style.strokeDashoffset = totalLength - progress;

            if (timeLeft <= 25 * 30) {
                tree.style.opacity = 0.6; // Half-grown 🌿
            }
            if (timeLeft <= 10) {
                tree.style.opacity = 1; // Full-grown 🌳
            }
        }

        function startTimer() {
            if (!running) {
                running = true;
                tree.style.opacity = 0.2;
                interval = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(interval);
                        alert("🎉 Pomodoro complete! Your tree has fully grown! 🌳");
                        running = false;
                        tree.style.opacity = 1;
                    }
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(interval);
            timeLeft = 25 * 60;
            running = false;
            tree.style.opacity = 0.2;
            updateTimerDisplay();
        }

        startButton.addEventListener("click", startTimer);
        resetButton.addEventListener("click", resetTimer);
        updateTimerDisplay();
    </script>

</body>
</html>
