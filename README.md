<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>動体視力チャレンジ</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: "Arial", "Noto Sans JP", sans-serif;
  background: linear-gradient(135deg, #17172b, #29295a);
  color: white;
  min-height: 100vh;
}

header {
  text-align: center;
  padding: 30px 15px 20px;
}

header h1 {
  margin: 0;
  font-size: 38px;
}

header p {
  margin-top: 10px;
  color: #cfd0ff;
}

.container {
  width: min(950px, 94%);
  margin: auto;
}

.menu {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  margin-bottom: 18px;
}

.menu button {
  border: none;
  border-radius: 12px;
  padding: 14px 8px;
  background: #3b3b72;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
}

.menu button:hover {
  background: #5555a0;
  transform: translateY(-2px);
}

.menu button.active {
  background: #6c63ff;
}

.panel {
  background: rgba(255,255,255,0.09);
  border: 1px solid rgba(255,255,255,0.15);
  border-radius: 20px;
  padding: 25px;
  box-shadow: 0 15px 40px rgba(0,0,0,0.25);
}

.game-title {
  text-align: center;
  font-size: 25px;
  margin-top: 0;
}

.description {
  text-align: center;
  color: #d5d5e8;
}

.stats {
  display: flex;
  justify-content: space-around;
  gap: 10px;
  margin: 20px 0;
}

.stat {
  background: rgba(0,0,0,0.2);
  padding: 12px;
  border-radius: 12px;
  text-align: center;
  flex: 1;
}

.stat span {
  display: block;
  font-size: 25px;
  font-weight: bold;
  color: #8ef6ff;
}

button.main {
  display: block;
  margin: 20px auto;
  padding: 14px 35px;
  border: none;
  border-radius: 30px;
  background: linear-gradient(90deg, #6c63ff, #00c6ff);
  color: white;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
}

button.main:hover {
  transform: scale(1.04);
}

#gameArea {
  position: relative;
  height: 430px;
  background: #10101e;
  border-radius: 18px;
  overflow: hidden;
  border: 2px solid #44446d;
}

.ball {
  position: absolute;
  width: 45px;
  height: 45px;
  border-radius: 50%;
  cursor: pointer;
  user-select: none;
  box-shadow: 0 0 20px currentColor;
}

.target {
  background: #ff3b6b;
  color: #ff3b6b;
}

.fake {
  background: #36e0ff;
  color: #36e0ff;
}

.big-message {
  position: absolute;
  inset: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 90px;
  font-weight: bold;
}

.number-input {
  display: block;
  margin: 20px auto;
  width: 180px;
  padding: 13px;
  border-radius: 12px;
  border: 2px solid #666;
  font-size: 25px;
  text-align: center;
}

.result {
  text-align: center;
  font-size: 22px;
  margin: 20px;
  min-height: 35px;
}

.records {
  margin-top: 30px;
}

.records h3 {
  text-align: center;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

th, td {
  padding: 10px;
  border-bottom: 1px solid #45455e;
  text-align: center;
}

th {
  color: #8ef6ff;
}

.info {
  line-height: 1.8;
  color: #ddd;
}

.hidden {
  display: none;
}

footer {
  text-align: center;
  padding: 30px;
  color: #999;
}

@media (max-width: 650px) {
  header h1 {
    font-size: 29px;
  }

  .menu {
    grid-template-columns: repeat(2, 1fr);
  }

  #gameArea {
    height: 360px;
  }

  .stats {
    flex-direction: column;
  }
}
</style>
</head>

<body>

<header>
  <h1>👀 動体視力チャレンジ</h1>
  <p>あなたの「見る力」をゲームでチェック！</p>
</header>

<div class="container">

  <div class="menu">
    <button class="active" onclick="showGame('catch')">🎯 ボール</button>
    <button onclick="showGame('number')">🔢 数字</button>
    <button onclick="showGame('find')">👀 探せ！</button>
    <button onclick="showGame('record')">📊 記録</button>
  </div>

  <!-- ボールゲーム -->
  <section id="catch" class="panel">
    <h2 class="game-title">🎯 逃げるボールを捕まえろ！</h2>

    <p class="description">
      赤いボールをできるだけ早くクリックしよう！<br>
      成功するほどスピードが上がる！
    </p>

    <div class="stats">
      <div class="stat">
        スコア
        <span id="catchScore">0</span>
      </div>

      <div class="stat">
        レベル
        <span id="catchLevel">1</span>
      </div>

      <div class="stat">
        残り
        <span id="catchLeft">10</span>
      </div>
    </div>

    <div id="catchArea" style="
      position:relative;
      height:430px;
      background:#10101e;
      border-radius:18px;
      overflow:hidden;
      border:2px solid #44446d;">
    </div>

    <button class="main" onclick="startCatch()">スタート！</button>

    <div id="catchResult" class="result"></div>
  </section>


  <!-- 数字ゲーム -->
  <section id="number" class="panel hidden">
    <h2 class="game-title">🔢 一瞬数字チャレンジ</h2>

    <p class="description">
      一瞬だけ表示される数字を覚えて答えよう！
    </p>

    <div id="numberArea" style="
      position:relative;
      height:300px;
      background:#10101e;
      border-radius:18px;
      overflow:hidden;">
    </div>

    <input
      id="numberInput"
      class="number-input"
      type="number"
      placeholder="数字を入力"
      disabled>

    <button class="main" onclick="startNumber()">スタート！</button>

    <div id="numberResult" class="result"></div>
  </section>


  <!-- 探すゲーム -->
  <section id="find" class="panel hidden">
    <h2 class="game-title">👀 違う動きを探せ！</h2>

    <p class="description">
      動いているボールの中から<br>
      「動き方が違うボール」をクリック！
    </p>

    <div id="findArea" style="
      position:relative;
      height:430px;
      background:#10101e;
      border-radius:18px;
      overflow:hidden;">
    </div>

    <button class="main" onclick="startFind()">スタート！</button>

    <div id="findResult" class="result"></div>
  </section>


  <!-- 記録 -->
  <section id="record" class="panel hidden">

    <h2 class="game-title">📊 あなたの研究記録</h2>

    <p class="description">
      ゲームの最高スコアを記録して、変化を観察しよう！
    </p>

    <div class="records">

      <h3>🎯 ボールゲーム</h3>

      <table>
        <thead>
          <tr>
            <th>回数</th>
            <th>スコア</th>
            <th>レベル</th>
          </tr>
        </thead>

        <tbody id="recordTable">
        </tbody>
      </table>

    </div>

    <button class="main" onclick="clearRecords()">
      記録をリセット
    </button>

    <hr>

    <div class="info">

      <h3>🔬 自由研究のヒント</h3>

      <p>
        例えば「毎日5分間練習するとゲームの成績は変化するのか？」
        というテーマで調べることができます。
      </p>

      <p>
        同じゲームを同じ条件で何日か行い、
        スコアの変化を記録してグラフにすると、
        練習による変化を観察しやすくなります。
      </p>

      <p>
        ※ゲームのスコアが上がることと、
        実生活での動体視力が向上することは同じではありません。
      </p>

    </div>

  </section>

</div>

<footer>
  動体視力チャレンジ ｜ 自由研究プロジェクト
</footer>


<script>

/* =========================
   共通
========================= */

function showGame(name) {

  const sections = ["catch", "number", "find", "record"];

  sections.forEach(id => {
    document.getElementById(id).classList.add("hidden");
  });

  document.getElementById(name).classList.remove("hidden");

  document.querySelectorAll(".menu button").forEach(btn => {
    btn.classList.remove("active");
  });

  const buttons = document.querySelectorAll(".menu button");

  const index = sections.indexOf(name);

  if(index >= 0) {
    buttons[index].classList.add("active");
  }

  if(name === "record") {
    displayRecords();
  }
}


/* =========================
   🎯 ボールゲーム
========================= */

let catchScore = 0;
let catchLeft = 10;
let catchLevel = 1;
let catchTimer = null;

function startCatch() {

  clearTimeout(catchTimer);

  catchScore = 0;
  catchLeft = 10;
  catchLevel = 1;

  updateCatch();

  document.getElementById("catchResult").textContent = "";

  nextCatchBall();
}

function updateCatch() {

  document.getElementById("catchScore").textContent = catchScore;
  document.getElementById("catchLeft").textContent = catchLeft;
  document.getElementById("catchLevel").textContent = catchLevel;
}

function nextCatchBall() {

  const area = document.getElementById("catchArea");

  area.innerHTML = "";

  if(catchLeft <= 0) {

    const result =
      "🏆 終了！ スコア " +
      catchScore +
      "点 / レベル " +
      catchLevel;

    document.getElementById("catchResult").textContent = result;

    saveRecord(catchScore, catchLevel);

    return;
  }

  const ball = document.createElement("div");

  ball.className = "ball target";

  const maxX = area.clientWidth - 45;
  const maxY = area.clientHeight - 45;

  ball.style.left = Math.random() * maxX + "px";
  ball.style.top = Math.random() * maxY + "px";

  area.appendChild(ball);

  let x = parseFloat(ball.style.left);
  let y = parseFloat(ball.style.top);

  let dx = (Math.random() > 0.5 ? 1 : -1)
         * (2 + catchLevel * 0.8);

  let dy = (Math.random() > 0.5 ? 1 : -1)
         * (2 + catchLevel * 0.8);

  let running = true;

  function move() {

    if(!running) return;

    x += dx;
    y += dy;

    if(x <= 0 || x >= maxX) dx *= -1;
    if(y <= 0 || y >= maxY) dy *= -1;

    ball.style.left = x + "px";
    ball.style.top = y + "px";

    requestAnimationFrame(move);
  }

  move();

  ball.onclick = function() {

    if(!running) return;

    running = false;

    catchScore += 10;
    catchLeft--;

    if(catchLeft % 3 === 0) {
      catchLevel++;
    }

    updateCatch();

    nextCatchBall();
  };

  catchTimer = setTimeout(() => {

    if(!running) return;

    running = false;

    catchLeft--;

    updateCatch();

    nextCatchBall();

  }, Math.max(700, 2300 - catchLevel * 180));
}


/* =========================
   🔢 数字ゲーム
========================= */

let numberAnswer = "";
let numberRound = 0;

function startNumber() {

  numberRound = 0;

  document.getElementById("numberInput").disabled = false;
  document.getElementById("numberResult").textContent = "";

  nextNumber();
}

function nextNumber() {

  if(numberRound >= 5) {

    document.getElementById("numberResult").textContent =
      "🎉 5問終了！よく見えたね！";

    document.getElementById("numberInput").disabled = true;

    return;
  }

  numberRound++;

  const area = document.getElementById("numberArea");

  area.innerHTML = "";

  numberAnswer =
    Math.floor(10 + Math.random() * 90).toString();

  const text = document.createElement("div");

  text.className = "big-message";

  text.textContent = numberAnswer;

  area.appendChild(text);

  setTimeout(() => {

    text.remove();

    document.getElementById("numberInput").value = "";

    document.getElementById("numberInput").focus();

  }, Math.max(250, 900 - numberRound * 100));
}


document.getElementById("numberInput").addEventListener(
  "keydown",
  function(e) {

    if(e.key !== "Enter") return;

    const value = this.value;

    if(value === numberAnswer) {

      document.getElementById("numberResult").textContent =
        "⭕ 正解！";

    } else {

      document.getElementById("numberResult").textContent =
        "❌ 正解は " + numberAnswer + " でした";
    }

    setTimeout(nextNumber, 700);
  }
);


/* =========================
   👀 違う動きを探す
========================= */

let findRunning = false;

function startFind() {

  const area = document.getElementById("findArea");

  area.innerHTML = "";

  document.getElementById("findResult").textContent = "";

  findRunning = true;

  const balls = [];

  for(let i = 0; i < 9; i++) {

    const ball = document.createElement("div");

    ball.className = "ball fake";

    const x = Math.random() *
      (area.clientWidth - 45);

    const y = Math.random() *
      (area.clientHeight - 45);

    ball.style.left = x + "px";
    ball.style.top = y + "px";

    area.appendChild(ball);

    balls.push({
      el: ball,
      x: x,
      y: y,
      dx: (Math.random() > 0.5 ? 1 : -1) * 1.5,
      dy: (Math.random() > 0.5 ? 1 : -1) * 1.5
    });
  }

  const targetIndex =
    Math.floor(Math.random() * balls.length);

  const target = balls[targetIndex];

  target.el.className = "ball target";

  target.el.onclick = function() {

    if(!findRunning) return;

    findRunning = false;

    document.getElementById("findResult").textContent =
      "🎉 正解！違う動きを見つけた！";

    setTimeout(() => {
      area.innerHTML = "";
    }, 700);
  };

  function animate() {

    if(!findRunning) return;

    balls.forEach((b, index) => {

      if(index === targetIndex) {

        b.x += b.dx * 2.8;
        b.y += b.dy * 2.8;

      } else {

        b.x += b.dx;
        b.y += b.dy;
      }

      const maxX = area.clientWidth - 45;
      const maxY = area.clientHeight - 45;

      if(b.x < 0 || b.x > maxX) b.dx *= -1;
      if(b.y < 0 || b.y > maxY) b.dy *= -1;

      b.el.style.left = b.x + "px";
      b.el.style.top = b.y + "px";
    });

    requestAnimationFrame(animate);
  }

  animate();
}


/* =========================
   📊 記録
========================= */

function saveRecord(score, level) {

  let records =
    JSON.parse(localStorage.getItem("visionRecords") || "[]");

  records.push({
    date: new Date().toLocaleString("ja-JP"),
    score: score,
    level: level
  });

  localStorage.setItem(
    "visionRecords",
    JSON.stringify(records)
  );
}


function displayRecords() {

  const table =
    document.getElementById("recordTable");

  table.innerHTML = "";

  let records =
    JSON.parse(localStorage.getItem("visionRecords") || "[]");

  records.slice().reverse().forEach((record, index) => {

    const row = document.createElement("tr");

    row.innerHTML =
      "<td>" + (index + 1) + "</td>" +
      "<td>" + record.score + "</td>" +
      "<td>" + record.level + "</td>";

    table.appendChild(row);

  });

  if(records.length === 0) {

    table.innerHTML =
      "<tr><td colspan='3'>まだ記録がありません</td></tr>";
  }
}


function clearRecords() {

  if(confirm("記録を全部消しますか？")) {

    localStorage.removeItem("visionRecords");

    displayRecords();
  }
}

</script>

</body>
</html>
