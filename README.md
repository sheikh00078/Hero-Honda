<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <title>হোম - Hero Honda</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: url('https://i.imgur.com/y1YgGCe.jpg') no-repeat center center fixed;
      background-size: cover;
      color: white;
    }
    .container {
      max-width: 500px;
      margin: 50px auto;
      background: rgba(0, 0, 0, 0.7);
      padding: 25px;
      border-radius: 15px;
      text-align: center;
    }
    h2 {
      margin-bottom: 20px;
    }
    button {
      padding: 12px 20px;
      margin: 10px 5px;
      font-size: 16px;
      background-color: #28a745;
      border: none;
      color: white;
      border-radius: 8px;
      cursor: pointer;
    }
    .section {
      margin-top: 20px;
      display: none;
      text-align: left;
    }
    input {
      width: 95%;
      padding: 10px;
      margin-top: 10px;
      border-radius: 5px;
      border: none;
    }
    .balance {
      font-size: 20px;
      margin-top: 30px;
      background: #222;
      padding: 15px;
      border-radius: 10px;
    }
  </style>
</head>
<body>

<div class="container">
  <h2>🏠 হোম - Hero Honda</h2>

  <button onclick="toggleSection('recharge')">💰 টাকা রিচার্জ</button>
  <button onclick="toggleSection('withdraw')">📤 উত্তোলন</button>
  <button onclick="alert('যোগাযোগ: ০১৩০১৮১৩৯৮১')">📞 গ্রাহক সেবা</button>

  <div class="section" id="recharge">
    <h3>বিকাশ নাম্বার: ০১৩০১৮১৩৯৮১</h3>
    <input type="text" placeholder="ট্রানজেকশন আইডি দিন" id="trxId">
    <button onclick="verifyRecharge()">রিচার্জ ভেরিফাই</button>
  </div>

  <div class="section" id="withdraw">
    <input type="text" placeholder="আপনার নাম" id="wName">
    <input type="text" placeholder="বিকাশ নাম্বার" id="wNumber">
    <input type="number" placeholder="টাকার পরিমাণ" id="wAmount">
    <button onclick="withdrawMoney()">উত্তোলন করুন</button>
  </div>

  <div class="balance">
    🔐 একাউন্ট ব্যালেন্স: <span id="balance">৳0</span><br><br>
    <button onclick="claimReward()">🎁 লগ ইন পুরস্কার</button>
  </div>
</div>

<script>
  // অ্যাকাউন্ট ব্যালেন্স সিমুলেশন
  let balance = localStorage.getItem("heroBalance") ? parseFloat(localStorage.getItem("heroBalance")) : 0;
  document.getElementById("balance").innerText = "৳" + balance;

  function toggleSection(id) {
    document.getElementById("recharge").style.display = "none";
    document.getElementById("withdraw").style.display = "none";
    document.getElementById(id).style.display = "block";
  }

  function verifyRecharge() {
    const trx = document.getElementById("trxId").value;
    if (trx.length < 5) {
      alert("সঠিক ট্রানজেকশন আইডি দিন");
    } else {
      balance += 100;
      localStorage.setItem("heroBalance", balance);
      document.getElementById("balance").innerText = "৳" + balance;
      alert("রিচার্জ সফল!");
    }
  }

  function withdrawMoney() {
    const name = document.getElementById("wName").value;
    const number = document.getElementById("wNumber").value;
    const amount = parseFloat(document.getElementById("wAmount").value);

    if (!name || !number || !amount) {
      alert("সব তথ্য দিন");
      return;
    }

    if (amount > balance) {
      alert("পর্যাপ্ত ব্যালেন্স নেই");
      return;
    }

    balance -= amount;
    localStorage.setItem("heroBalance", balance);
    document.getElementById("balance").innerText = "৳" + balance;
    alert(`৳${amount} উত্তোলনের অনুরোধ গ্রহণ হয়েছে`);
  }

  function claimReward() {
    const lastClaim = localStorage.getItem("lastClaimTime");
    const now = Date.now();

    if (!lastClaim || now - parseInt(lastClaim) > 24 * 60 * 60 * 1000) {
      balance += 10;
      localStorage.setItem("heroBalance", balance);
      localStorage.setItem("lastClaimTime", now.toString());
      document.getElementById("balance").innerText = "৳" + balance;
      alert("১০ টাকা পুরস্কার যুক্ত হয়েছে!");
    } else {
      const remaining = 24 - Math.floor((now - parseInt(lastClaim)) / (60 * 60 * 1000));
      alert(remaining + " ঘণ্টা পর আবার চেষ্টা করুন");
    }
  }
</script>

</body>
</html>
