<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8" />
  <title>Clicker</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #111;
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    #game-container {
      display: flex;
      gap: 40px;
      align-items: center;
      justify-content: center;
    }

    #main {
      text-align: center;
    }

    #points {
      font-size: 28px;
      margin-bottom: 20px;
    }

    #red-button {
      width: 150px;
      height: 150px;
      border-radius: 50%;
      border: none;
      background: red;
      box-shadow: 0 0 20px rgba(255, 0, 0, 0.7);
      cursor: pointer;
      font-size: 20px;
      color: #fff;
      font-weight: bold;
      transition: transform 0.1s, box-shadow 0.1s;
    }

    #red-button:active {
      transform: scale(0.95);
      box-shadow: 0 0 5px rgba(255, 0, 0, 0.5);
    }

    #shop {
      background: #222;
      padding: 20px;
      border-radius: 10px;
      width: 260px;
    }

    #shop h2 {
      margin-top: 0;
      text-align: center;
    }

    .upgrade {
      border-bottom: 1px solid #444;
      padding: 10px 0;
    }

    .upgrade:last-child {
      border-bottom: none;
    }

    .upgrade button {
      margin-top: 5px;
      padding: 5px 10px;
      cursor: pointer;
      border: none;
      border-radius: 5px;
      background: #4caf50;
      color: #fff;
      font-weight: bold;
    }

    .upgrade button:disabled {
      background: #555;
      cursor: not-allowed;
    }
  </style>
</head>
<body>
  <div id="game-container">
    <div id="main">
      <div id="points">Punkty: <span id="points-value">0</span></div>
      <button id="red-button">KLIKNIJ</button>
      <div style="margin-top:10px;font-size:14px;">
        Punkty na klik: <span id="ppc">1</span>
      </div>
    </div>

    <div id="shop">
      <h2>Sklep</h2>

      <div class="upgrade">
        <div><strong>Więcej punktów za klik</strong></div>
        <div>Koszt: <span id="cost-ppc">20</span> pkt</div>
        <div>Poziom: <span id="level-ppc">0</span></div>
        <button id="buy-ppc">Kup</button>
      </div>

      <div class="upgrade">
        <div><strong>Auto-kliker</strong></div>
        <div>Koszt: <span id="cost-auto">50</span> pkt</div>
        <div>Poziom: <span id="level-auto">0</span></div>
        <button id="buy-auto">Kup</button>
      </div>
    </div>
  </div>

  <script>
    let points = 0;
    let pointsPerClick = 1;

    let ppcLevel = 0;
    let ppcBaseCost = 20;

    let autoLevel = 0;
    let autoBaseCost = 50;
    let autoInterval = null;

    const pointsValueEl = document.getElementById("points-value");
    const ppcEl = document.getElementById("ppc");
    const redButton = document.getElementById("red-button");

    const costPpcEl = document.getElementById("cost-ppc");
    const levelPpcEl = document.getElementById("level-ppc");
    const buyPpcBtn = document.getElementById("buy-ppc");

    const costAutoEl = document.getElementById("cost-auto");
    const levelAutoEl = document.getElementById("level-auto");
    const buyAutoBtn = document.getElementById("buy-auto");

    function updateUI() {
      pointsValueEl.textContent = points;
      ppcEl.textContent = pointsPerClick;

      const ppcCost = getPpcCost();
      const autoCost = getAutoCost();

      costPpcEl.textContent = ppcCost;
      levelPpcEl.textContent = ppcLevel;

      costAutoEl.textContent = autoCost;
      levelAutoEl.textContent = autoLevel;

      buyPpcBtn.disabled = points < ppcCost;
      buyAutoBtn.disabled = points < autoCost;
    }

    function getPpcCost() {
      return Math.floor(ppcBaseCost * Math.pow(1.5, ppcLevel));
    }

    function getAutoCost() {
      return Math.floor(autoBaseCost * Math.pow(1.7, autoLevel));
    }

    redButton.addEventListener("click", () => {
      points += pointsPerClick;
      updateUI();
    });

    buyPpcBtn.addEventListener("click", () => {
      const cost = getPpcCost();
      if (points >= cost) {
        points -= cost;
        ppcLevel++;
        pointsPerClick += 1; // +1 punkt na klik za poziom
        updateUI();
      }
    });

    buyAutoBtn.addEventListener("click", () => {
      const cost = getAutoCost();
      if (points >= cost) {
        points -= cost;
        autoLevel++;
        startAutoClicker();
        updateUI();
      }
    });

    function startAutoClicker() {
      if (autoInterval) clearInterval(autoInterval);
      // Im wyższy poziom, tym więcej punktów na sekundę
      autoInterval = setInterval(() => {
        points += autoLevel; // 1 punkt na poziom na sekundę
        updateUI();
      }, 1000);
    }

    updateUI();
  </script>
</body>
</html>
