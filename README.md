<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>OptiTrade Wallet (Offline)</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      background-color: #0f172a;
      color: white;
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }
    .btn {
      padding: 10px 20px;
      margin: 10px;
      background: #10b981;
      color: black;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    input {
      padding: 10px;
      width: 90%;
      margin: 5px 0;
      border-radius: 5px;
      border: none;
    }
    .box {
      background: #1e293b;
      padding: 20px;
      margin-top: 20px;
      border-radius: 10px;
    }
    code {
      background: #334155;
      padding: 6px 10px;
      border-radius: 5px;
      display: inline-block;
      word-break: break-word;
    }
  </style>
</head>
<body>
  <h1>OptiTrade Кошелёк</h1>
  <p>Локальный кошелёк без MetaMask</p>

  <button class="btn" onclick="generateWallet()">🎲 Сгенерировать адрес</button>
  <div class="box">
    <p><strong>Адрес:</strong><br><code id="address">–</code></p>
    <p><strong>Баланс OPT:</strong> <span id="balance">0</span> OPT</p>
  </div>

  <div class="box">
    <h3>📤 Отправка OPT</h3>
    <input type="text" id="toAddress" placeholder="Адрес получателя"><br>
    <input type="number" id="amount" placeholder="Сумма в OPT"><br>
    <button class="btn" onclick="sendOPT()">Отправить</button>
  </div>

  <div class="box">
    <h3>📜 История</h3>
    <ul id="history" style="text-align:left; padding-left:20px;"></ul>
  </div>

  <script>
    let wallet = { address: "", balance: 100 };

    function generateWallet() {
      const random = crypto.getRandomValues(new Uint8Array(20));
      wallet.address = "0x" + Array.from(random).map(b => b.toString(16).padStart(2, '0')).join('');
      wallet.balance = 100;
      document.getElementById("address").innerText = wallet.address;
      document.getElementById("balance").innerText = wallet.balance.toFixed(2);
      document.getElementById("history").innerHTML = "";
    }

    function sendOPT() {
      const to = document.getElementById("toAddress").value;
      const amount = parseFloat(document.getElementById("amount").value);
      if (!to || isNaN(amount) || amount <= 0) return alert("Введите корректные данные.");
      if (amount > wallet.balance) return alert("Недостаточно средств.");

      wallet.balance -= amount;
      document.getElementById("balance").innerText = wallet.balance.toFixed(2);
      const entry = `<li>→ ${to}: ${amount.toFixed(2)} OPT</li>`;
      document.getElementById("history").innerHTML += entry;
    }
  </script>
</body>
</html>
