<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pomodoro Forest</title>
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
            background-color: #ffe4e1;
        }

        h1 {
            color: #ff69b4;
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

        #tree {
            width: 120px;
            margin-top: 20px;
            transition: opacity 0.5s ease-in-out;
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

    <h1>🎀 Pomodoro Forest 🎀</h1>
    <div class="timer-container">
        <svg width="200" height="200">
            <circle cx="100" cy="100" r="90" stroke="#ff69b4" stroke-width="10" fill="none" class="circle"/>
        </svg>
        <div class="time" id="timer">25:00</div>
    </div>

    <img id="tree" src="https://i.imgur.com/k6URj6K.png" alt="Tree" style="opacity: 0.2;">
    
    <button id="start">Start</button>
    <button id="reset">Reset</button>

    <script>
        let timerDisplay = document.getElementById("timer");
        let startButton = document.getElementById("start");
        let resetButton = document.getElementById("reset");
        let tree = document.getElementById("tree");
        let timeLeft = 25 * 60;
        let interval;
        let running = false;
        let totalLength = 2 * Math.PI * 90;
        let circle = document.querySelector(".circle");

        function updateTimerDisplay() {
            let minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;
            timerDisplay.innerHTML = `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
            let progress = (timeLeft / (25 * 60)) * totalLength;
            circle.style.strokeDashoffset = totalLength - progress;
        }

        function startTimer() {
            if (!running) {
                running = true;
                tree.style.opacity = 1; // Tree starts growing
                interval = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(interval);
                        alert("🍓 Pomodoro complete! Your tree has grown! 🌳");
                        running = false;
                        saveToNotion(true);
                    }
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(interval);
            timeLeft = 25 * 60;
            running = false;
            tree.style.opacity = 0.2; // Tree disappears if reset
            updateTimerDisplay();
            saveToNotion(false);
        }

        function saveToNotion(completed) {
            fetch("https://api.notion.com/v1/pages", {
                method: "POST",
                headers: {
                    "Authorization": "Bearer secret_xxxxxx",  // Replace with your Notion API Key
                    "Notion-Version": "2022-06-28",
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({
                    "parent": { "database_id": "your_database_id" },  // Replace with your Notion DB ID
                    "properties": {
                        "Title": { "title": [{ "text": { "content": "Pomodoro Session" } }] },
                        "Status": { "select": { "name": completed ? "Completed" : "Abandoned" } },
                        "Date": { "date": { "start": new Date().toISOString() } }
                    }
                })
            }).then(response => {
                if (response.ok) {
                    console.log("✅ Pomodoro saved to Notion!");
                } else {
                    console.log("❌ Failed to save:", response.statusText);
                }
            });
        }

        startButton.addEventListener("click", startTimer);
        resetButton.addEventListener("click", resetTimer);
        updateTimerDisplay();
    </script>

</body>
</html>
