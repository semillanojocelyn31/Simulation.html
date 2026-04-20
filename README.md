<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BSIT-2B Attendance System</title>

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

/* ANIMATED GRADIENT */
body::before {
    content: "";
    position: fixed;
    width: 200%;
    height: 200%;
    background: linear-gradient(-45deg, #ff00cc, #ff66cc, #6a00ff, #ff00cc);
    background-size: 400% 400%;
    animation: gradientMove 12s ease infinite;
    opacity: 0.25;
    z-index: -2;
}

@keyframes gradientMove {
    0% { transform: translate(0,0); }
    50% { transform: translate(-25%, -25%); }
    100% { transform: translate(0,0); }
}

/* FLOATING BLOBS */
body::after {
    content: "";
    position: fixed;
    width: 100%;
    height: 100%;
    background:
        radial-gradient(circle, rgba(255,0,200,0.4) 0%, transparent 60%) 20% 30%,
        radial-gradient(circle, rgba(255,102,204,0.3) 0%, transparent 60%) 70% 60%,
        radial-gradient(circle, rgba(106,0,255,0.3) 0%, transparent 60%) 50% 80%;
    animation: floatBlobs 15s infinite alternate ease-in-out;
    z-index: -1;
}

@keyframes floatBlobs {
    0% { transform: translateY(0px) translateX(0px); }
    50% { transform: translateY(-40px) translateX(20px); }
    100% { transform: translateY(20px) translateX(-20px); }
}

/* SPARKLES */
.sparkle {
    position: fixed;
    width: 2px;
    height: 2px;
    background: white;
    box-shadow: 0 0 8px white, 0 0 12px #ff66cc;
    animation: sparkleAnim 3s infinite ease-in-out;
}

@keyframes sparkleAnim {
    0%,100% { opacity: 0.2; transform: scale(1); }
    50% { opacity: 1; transform: scale(2); }
}

header { text-align: center; margin-bottom: 20px; }

h1 {
    text-shadow: 0 0 20px #ff7eb3, 0 0 40px #ff00cc, 0 0 70px #ff66cc;
    font-size: 2.7rem;
}

.controls-top {
    display: flex;
    gap: 15px;
    margin-bottom: 25px;
    background: rgba(255, 255, 255, 0.12);
    padding: 15px;
    border-radius: 20px;
    backdrop-filter: blur(15px);
    border: 1px solid rgba(255, 0, 200, 0.6);
    box-shadow: 0 0 25px rgba(255,0,200,0.6);
}

input[type="date"], .subject-input {
    background: rgba(0, 0, 0, 0.5);
    border: 1px solid #ff00cc;
    color: white;
    padding: 10px;
    border-radius: 10px;
    outline: none;
    box-shadow: 0 0 12px #ff00cc, 0 0 20px #ff66cc;
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
    background: rgba(255, 255, 255, 0.08);
    backdrop-filter: blur(18px);
    border-radius: 25px;
    padding: 20px;
    width: 350px;
    border: 1px solid rgba(255, 0, 200, 0.4);
    box-shadow: 0 0 30px rgba(255,0,200,0.5);
}

.section h2 {
    text-align: center;
    color: #ff9de6;
    text-shadow: 0 0 15px #ff00cc, 0 0 30px #ff66cc;
}

.member-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px;
    margin: 8px 0;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 15px;
    transition: 0.3s;
}

.member-row:hover {
    background: rgba(255, 0, 200, 0.25);
    transform: scale(1.05);
    box-shadow: 0 0 20px #ff00cc, 0 0 35px #ff66cc;
}

.switch { position: relative; display: inline-block; width: 40px; height: 20px; }
.switch input { opacity: 0; width: 0; height: 0; }

.slider {
    position: absolute;
    cursor: pointer;
    top: 0; left: 0; right: 0; bottom: 0;
    background: #555;
    transition: .4s;
    border-radius: 20px;
}

.slider:before {
    position: absolute;
    content: "";
    height: 14px;
    width: 14px;
    left: 3px;
    bottom: 3px;
    background: white;
    transition: .4s;
    border-radius: 50%;
}

input:checked + .slider {
    background: #ff00cc;
    box-shadow: 0 0 20px #ff00cc, 0 0 40px #ff66cc;
}

input:checked + .slider:before {
    transform: translateX(20px);
}

.done-container { margin-top: 30px; }

.done-btn {
    background: linear-gradient(90deg, #ff00cc, #ff66cc, #ff00cc);
    color: white;
    border: none;
    padding: 16px 90px;
    border-radius: 50px;
    font-size: 1.3rem;
    font-weight: bold;
    cursor: pointer;
    box-shadow: 0 0 30px rgba(255, 0, 200, 0.9);
}

#summaryPanel {
    width: 100%;
    max-width: 900px;
    background: rgba(20, 0, 20, 0.85);
    border-radius: 25px;
    padding: 25px;
    display: none;
    border: 2px solid #ff00cc;
    margin-top: 20px;
    box-shadow: 0 0 35px #ff00cc, 0 0 60px #ff66cc;
}

.stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
}

.stat-box {
    background: rgba(255, 255, 255, 0.08);
    padding: 18px;
    border-radius: 20px;
}

.present-text {
    color: #ffb3f0;
    text-shadow: 0 0 15px #ff00cc;
}

.absent-text {
    color: #ff6699;
    text-shadow: 0 0 15px #ff4d88;
}

.name-list {
    font-size: 0.9rem;
    line-height: 1.8;
    margin-top: 10px;
    white-space: pre-line;
}

html { scroll-behavior: smooth; }
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

<div class="done-container">
    <button class="done-btn" onclick="generateSummary()">DONE ✨</button>
</div>

<div id="summaryPanel">
    <h2 style="text-align:center; color:#ff7eb3;">Attendance Report</h2>
    <p id="reportMeta" style="text-align:center;"></p>

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

    document.getElementById('listPresent').innerText =
        present.length > 0 ? present.map(name => "• " + name).join("\n\n") : "None";

    document.getElementById('listAbsent').innerText =
        absent.length > 0 ? absent.map(name => "• " + name).join("\n\n") : "None";

    const dateVal = document.getElementById('attendanceDate').value;
    const subVal = document.getElementById('subjectName').value || "Class Session";
    document.getElementById('reportMeta').innerText = `${subVal} — ${dateVal}`;

    const panel = document.getElementById('summaryPanel');
    panel.style.display = 'block';
    panel.scrollIntoView({ behavior: 'smooth', block: 'start' });
}

/* SPARKLES */
for (let i = 0; i < 40; i++) {
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
