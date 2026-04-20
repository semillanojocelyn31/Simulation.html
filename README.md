<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BSIT-2B Attendance System</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        body {
            margin: 0;
            font-family: 'Poppins', sans-serif;
            background: radial-gradient(circle at top, #3a0a3f, #0f0015 70%);
            overflow-x: hidden;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            position: relative;
        }

        /* ANIMATED GRADIENT BACKGROUND */
        body::before {
            content: "";
            position: fixed;
            top: 0; left: 0;
            width: 200%; height: 200%;
            background: linear-gradient(-45deg, #ff00cc, #ff66cc, #6a00ff, #ff00cc);
            background-size: 400% 400%;
            animation: gradientMove 12s ease infinite;
            opacity: 0.15;
            z-index: -2;
        }

        @keyframes gradientMove {
            0% { transform: translate(0,0); }
            50% { transform: translate(-10%, -10%); }
            100% { transform: translate(0,0); }
        }

        /* SPARKLES */
        .sparkle {
            position: fixed;
            width: 2px;
            height: 2px;
            background: white;
            box-shadow: 0 0 8px white, 0 0 12px #ff66cc;
            animation: sparkleAnim 3s infinite ease-in-out;
            pointer-events: none;
        }

        @keyframes sparkleAnim {
            0%,100% { opacity: 0.2; transform: scale(1); }
            50% { opacity: 1; transform: scale(2); }
        }

        header h1 {
            text-shadow: 0 0 20px #ff7eb3, 0 0 40px #ff00cc;
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        .controls-top {
            display: flex;
            gap: 15px;
            margin-bottom: 25px;
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 20px;
            backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 0, 200, 0.5);
        }

        input[type="date"], .subject-input {
            background: rgba(0, 0, 0, 0.6);
            border: 1px solid #ff00cc;
            color: white;
            padding: 12px;
            border-radius: 10px;
            outline: none;
            font-family: 'Poppins', sans-serif;
        }

        .container {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
            justify-content: center;
            width: 100%;
            max-width: 1100px;
        }

        .section {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 20px;
            width: 340px;
            border: 1px solid rgba(255, 0, 200, 0.3);
        }

        .section h2 {
            text-align: center;
            color: #ff9de6;
            margin-top: 0;
        }

        .member-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            margin: 5px 0;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 12px;
            transition: 0.3s;
        }

        .member-row:hover {
            background: rgba(255, 0, 200, 0.2);
            transform: translateX(5px);
        }

        /* TOGGLE SWITCH */
        .switch { position: relative; display: inline-block; width: 44px; height: 22px; }
        .switch input { opacity: 0; width: 0; height: 0; }
        .slider {
            position: absolute; cursor: pointer;
            top: 0; left: 0; right: 0; bottom: 0;
            background: #444; transition: .4s; border-radius: 22px;
        }
        .slider:before {
            position: absolute; content: "";
            height: 16px; width: 16px; left: 3px; bottom: 3px;
            background: white; transition: .4s; border-radius: 50%;
        }
        input:checked + .slider { background: #ff00cc; box-shadow: 0 0 15px #ff00cc; }
        input:checked + .slider:before { transform: translateX(22px); }

        .done-btn {
            background: linear-gradient(90deg, #ff00cc, #6a00ff);
            color: white; border: none; padding: 15px 60px;
            border-radius: 50px; font-size: 1.2rem; font-weight: bold;
            cursor: pointer; margin-top: 30px; transition: 0.3s;
            box-shadow: 0 0 20px rgba(255, 0, 200, 0.6);
        }

        .done-btn:hover { transform: scale(1.05); box-shadow: 0 0 30px #ff00cc; }

        #summaryPanel {
            width: 90%; max-width: 800px;
            background: rgba(15, 0, 20, 0.95);
            border-radius: 20px; padding: 25px; display: none;
            border: 2px solid #ff00cc; margin-top: 30px;
            box-shadow: 0 0 40px rgba(255, 0, 204, 0.4);
            margin-bottom: 50px;
        }

        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 20px; }
        .stat-box { background: rgba(255, 255, 255, 0.05); padding: 15px; border-radius: 15px; }
        .present-text { color: #00ffcc; }
        .absent-text { color: #ff4d88; }
        .name-list { font-size: 0.85rem; white-space: pre-line; margin-top: 10px; opacity: 0.9; }

        .copy-btn {
            background: #333; color: white; border: 1px solid #ff00cc;
            padding: 12px 15px; border-radius: 10px; cursor: pointer;
            font-size: 0.9rem; margin-top: 15px; width: 100%;
        }
    </style>
</head>
<body>

<header>
    <h1>BSIT-2B Attendance</h1>
</header>

<div class="controls-top">
    <input type="date" id="attendanceDate">
    <input type="text" class="subject-input" id="subjectName" placeholder="Enter Subject Name">
</div>

<div class="container">
    <div class="section">
        <h2>Girls 🌸</h2>
        <div id="girlsList"></div>
    </div>
    <div class="section">
        <h2>Boys 💎</h2>
        <div id="boysList"></div>
    </div>
</div>

<button class="done-btn" onclick="generateSummary()">DONE ✨</button>

<div id="summaryPanel">
    <h2 style="text-align:center; color:#ff7eb3; margin-bottom:5px;">Attendance Summary</h2>
    <p id="reportMeta" style="text-align:center; opacity: 0.7; margin-bottom:20px;"></p>

    <div class="stats-grid">
        <div class="stat-box">
            <h3 class="present-text">Present: <span id="countPresent">0</span></h3>
            <div id="listPresent" class="name-list"></div>
        </div>
        <div class="stat-box">
            <h3 class="absent-text">Absent: <span id="countAbsent">0</span></h3>
            <div id="listAbsent" class="name-list"></div>
        </div>
    </div>
    <button class="copy-btn" onclick="copyToClipboard()">📋 Copy Absent List to Clipboard</button>
</div>

<script>
    const girls = ["Jocelyn Semillano","Andrea Jean Occeña","Gene Mae Caramihan","Jessa Erosido","Jessa Hilardino","Kristine Joy Basa","Lena Bahian","Rechelle An Casilangan","Roxan Pracio","Shannon De La Cruz","Jeca Dagumboy","Martina Cabrillos","Nera Bahilot","Sheila Marie Lañojan","Trisha Agravante"];
    const boys = ["Archie Vidal","John Rogen Bullag","Mario Nebres Jr.","AjhannAylle Marfil","Anthony Espinosa","Dave Rivera","Jeffrey Coriento","Kenneth Espinosa","Kevin James Plaza","Luke Axel Barrocum","Mark Angelou Banatasa","Nathaniel Pojas","Paul John Malba","Rodel Cepeda"];

    document.getElementById('attendanceDate').valueAsDate = new Date();

    function renderList(list, elementId) {
        const container = document.getElementById(elementId);
        list.sort().forEach(name => {
            container.innerHTML += `
            <div class="member-row">
                <span>${name}</span>
                <label class="switch">
                    <input type="checkbox" class="att-check" data-name="${name}">
                    <span class="slider"></span>
                </label>
            </div>`;
        });
    }

    renderList(girls, 'girlsList');
    renderList(boys, 'boysList');

    function generateSummary() {
        const checks = document.querySelectorAll('.att-check');
        let present = [];
        let absent = [];

        checks.forEach(check => {
            const name = check.getAttribute('data-name');
            if (check.checked) present.push(name);
            else absent.push(name);
        });

        document.getElementById('countPresent').innerText = present.length;
        document.getElementById('countAbsent').innerText = absent.length;
        document.getElementById('listPresent').innerText = present.length > 0 ? present.map(n => "• " + n).join("\n") : "None";
        document.getElementById('listAbsent').innerText = absent.length > 0 ? absent.map(n => "• " + n).join("\n") : "None";

        const dateVal = document.getElementById('attendanceDate').value;
        const subVal = document.getElementById('subjectName').value || "Class Session";
        document.getElementById('reportMeta').innerText = `${subVal} | ${dateVal}`;

        const panel = document.getElementById('summaryPanel');
        panel.style.display = 'block';
        panel.scrollIntoView({ behavior: 'smooth' });
    }

    function copyToClipboard() {
        const absentList = document.getElementById('listAbsent').innerText;
        const sub = document.getElementById('subjectName').value || "Class";
        const date = document.getElementById('attendanceDate').value;
        const text = `BSIT-2B Attendance\nSubject: ${sub}\nDate: ${date}\n\nAbsent Students:\n${absentList}`;
        
        navigator.clipboard.writeText(text).then(() => {
            alert("Report copied to clipboard!");
        });
    }

    for (let i = 0; i < 50; i++) {
        let star = document.createElement("div");
        star.className = "sparkle";
        star.style.top = Math.random() * 100 + "vh";
        star.style.left = Math.random() * 100 + "vw";
        star.style.animationDuration = (Math.random() * 3 + 2) + "s";
        document.body.appendChild(star);
    }
</script>

</body>
</html>
