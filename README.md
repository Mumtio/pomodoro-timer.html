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
            font-size: 26px;
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
            width: 80px;
            margin-top: 20px;
            transition: transform 1s ease-in-out;
            filter: drop-shadow(0px 6px 10px rgba(0, 0, 0, 0.2));
            display: block; /* Ensure visibility */
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

        .settings {
            margin-top: 15px;
            background: white;
            padding: 10px;
            border-radius: 15px;
            box-shadow: 0px 4px 10px rgba(255, 105, 180, 0.3);
        }

        select {
            border: none;
            background: #ff85a2;
            color: white;
            font-size: 16px;
            font-weight: bold;
            padding: 8px 12px;
            border-radius: 10px;
            cursor: pointer;
            margin-left: 10px;
        }
    </style>
</head>
<body>

    <h1>🎀 Pomodoro Forest 🎀</h1>

    <div class="settings">
        <label for="timeSelect">⏳ Choose Time: </label>
        <select id="timeSelect">
            <option value="25">25 min</option>
            <option value="15">15 min</option>
            <option value="10">10 min</option>
            <option value="5">5 min</option>
        </select>
    </div>

    <div class="timer-container">
        <svg width="220" height="220">
            <circle cx="110" cy="110" r="100" stroke="#ff69b4" stroke-width="10" fill="none" class="circle"/>
        </svg>
        <div class="time" id="timer">25:00</div>
    </div>

    <!-- FIXED: Using a working animated tree -->
    <img id="tree" src="https://upload.wikimedia.org/wikipedia/commons/3/36/Morphing_tree.gif" alt="Tree">
    
    <div class="button-container">
        <button id="start">🌱 Start</button>
        <button id="reset">💔 Reset</button>
    </div>

    <script>
        let timerDisplay = document.getElementById("timer");
        let startButton = document.getElementById("start");
        let resetButton = document.getElementById("reset");
        let timeSelect = document.getElementById("timeSelect");
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

            // Animate tree growth stages based on time left
            if (timeLeft >= timeLeft * 0.8) {
                tree.style.transform = "scale(0.5)"; // Tiny sprout 🌱
            }
            if (timeLeft < timeLeft * 0.6) {
                tree.style.transform = "scale(0.7)"; // Half-grown 🌿
            }
            if (timeLeft < timeLeft * 0.3) {
                tree.style.transform = "scale(1)"; // Fully grown 🌳
            }
        }

        function startTimer() {
            if (!running) {
                running = true;
                tree.style.transform = "scale(0.5)";
                interval = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(interval);
                        alert("🎉 Pomodoro complete! Your tree has fully grown! 🌳");
                        running = false;
                        tree.style.transform = "scale(1.2)";
                    }
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(interval);
            timeLeft = parseInt(timeSelect.value) * 60;
            running = false;
            tree.style.transform = "scale(0.5)"; // Reset to tiny sprout
            updateTimerDisplay();
        }

        timeSelect.addEventListener("change", () => {
            timeLeft = parseInt(timeSelect.value) * 60;
            updateTimerDisplay();
        });

        startButton.addEventListener("click", startTimer);
        resetButton.addEventListener("click", resetTimer);
        updateTimerDisplay();
    </script>

</body>
</html>
