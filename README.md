<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Timer 🌸</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&family=Pacifico&display=swap');

        :root {
            --bg-light: linear-gradient(135deg, #f8d7e8, #f0e2ff);
            --bg-dark: linear-gradient(135deg, #1e1e1e, #3a3a3a);
            --card-light: white;
            --card-dark: #444;
            --text-light: #ff7fa3;
            --text-dark: #ffd1dc;
            --water-light: #98d8f4;
            --water-dark: #2b6cb0;
        }

        * {
            font-family: "Quicksand", sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            transition: all 0.3s ease-in-out;
        }

        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: var(--bg-light);
        }

        .dark-mode {
            background: var(--bg-dark);
            color: var(--text-dark);
        }

        .card {
            background: var(--card-light);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0px 6px 15px rgba(0, 0, 0, 0.1);
            max-width: 350px;
            width: 90%;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .dark-mode .card {
            background: var(--card-dark);
        }

        .title {
            font-size: 22px;
            font-weight: bold;
            color: var(--text-light);
            font-family: "Pacifico", cursive;
            margin-bottom: 10px;
        }

        .dark-mode .title {
            color: var(--text-dark);
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
            overflow: hidden;
            border-radius: 50%;
            border: 4px solid var(--text-light);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .dark-mode .timer-container {
            border: 4px solid var(--text-dark);
        }

        .water {
            width: 200%;
            height: 0%;
            background: linear-gradient(to top, var(--water-light), #d1f3ff);
            position: absolute;
            bottom: 0;
            left: -50%;
            border-radius: 50%;
            animation: wave 2s infinite linear;
        }

        .dark-mode .water {
            background: linear-gradient(to top, var(--water-dark), #6095c2);
        }

        @keyframes wave {
            0% { transform: translateX(0px); }
            50% { transform: translateX(20px); }
            100% { transform: translateX(0px); }
        }

        .flip-card {
            perspective: 1000px;
            position: absolute;
            width: 100px;
            height: 50px;
        }

        .flip-card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            transform-style: preserve-3d;
            transition: transform 0.6s ease-in-out;
        }

        .flip-card-front, .flip-card-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 22px;
            font-weight: bold;
            color: var(--text-light);
            background: var(--card-light);
            border-radius: 10px;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
        }

        .dark-mode .flip-card-front, .dark-mode .flip-card-back {
            background: var(--card-dark);
            color: var(--text-dark);
        }

        .flip-card-back {
            transform: rotateY(180deg);
        }

        .flip {
            transform: rotateY(180deg);
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
            display: flex;
            gap: 10px;
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
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            box-shadow: 0px 4px 8px rgba(255, 105, 180, 0.3);
        }

        button:hover {
            transform: scale(1.1);
            box-shadow: 0px 6px 12px rgba(255, 105, 180, 0.5);
        }

        .toggle-mode {
            position: absolute;
            top: 10px;
            right: 20px;
            cursor: pointer;
            font-size: 18px;
        }
    </style>
</head>
<body>

    <div class="toggle-mode" onclick="toggleMode()">🌙</div>

    <div class="card">
        <div class="title">🌸 Pomodoro Timer 🌸</div>

        <div class="settings">
            ⏳ Time: <input type="number" id="customTime" value="25" min="1"> min
        </div>

        <div class="timer-container">
            <div class="water" id="water"></div>
            <div class="flip-card">
                <div class="flip-card-inner" id="flip-card">
                    <div class="flip-card-front" id="timer-front">25:00</div>
                    <div class="flip-card-back" id="timer-back">25:00</div>
                </div>
            </div>
        </div>

        <div class="button-container">
            <button id="start">Start</button>
            <button id="reset">Reset</button>
        </div>
    </div>

    <script>
        let timeLeft, interval, running = false;
        const timerFront = document.getElementById("timer-front");
        const timerBack = document.getElementById("timer-back");
        const flipCard = document.getElementById("flip-card");
        const startButton = document.getElementById("start");
        const resetButton = document.getElementById("reset");
        const customTime = document.getElementById("customTime");
        const water = document.getElementById("water");

        function updateTimer() {
            let minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;
            let displayTime = `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
            timerFront.innerText = timerBack.innerText = displayTime;
            flipCard.classList.toggle("flip");
            water.style.height = `${100 - (timeLeft / (customTime.value * 60)) * 100}%`;
        }

        startButton.onclick = () => { if (!running) running = !!(interval = setInterval(() => (timeLeft ? timeLeft-- : clearInterval(interval)), 1000)); };
        resetButton.onclick = () => location.reload();
        function toggleMode() { document.body.classList.toggle("dark-mode"); }
    </script>

</body>
</html>
