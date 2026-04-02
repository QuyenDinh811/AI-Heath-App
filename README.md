<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>AI Health Web</title>
  <style>
    body { font-family: Arial; margin:0; background:#f4f8fb; }
    header { background:#2b7de9; color:white; padding:15px; text-align:center; }
    nav { display:flex; justify-content:center; gap:10px; background:#fff; padding:10px; box-shadow:0 2px 5px rgba(0,0,0,0.1);} 
    nav button { padding:10px 15px; border:none; cursor:pointer; background:#e0e7ff; border-radius:5px; }
    .container { max-width:700px; margin:auto; padding:20px; }
    .card { background:white; padding:20px; border-radius:10px; margin-bottom:20px; box-shadow:0 2px 5px rgba(0,0,0,0.1);} 
    input, select, textarea, button { width:100%; padding:10px; margin-top:10px; }
    button { background:#2b7de9; color:white; }
    .hidden { display:none; }
  </style>
</head>
<body>

<header>
  <h1>AI Health Web</h1>
  <p>Theo dõi sức khỏe thông minh</p>
</header>

<nav>
  <button onclick="showPage('home')">Trang chủ</button>
  <button onclick="showPage('input')">Nhập dữ liệu</button>
  <button onclick="showPage('history')">Lịch sử</button>
  <button onclick="showPage('doctor')">Bác sĩ</button>
</nav>

<div class="container">

  <!-- HOME -->
  <div id="home" class="card">
    <h2>Giới thiệu</h2>
    <p>Ứng dụng giúp theo dõi sức khỏe hằng ngày, phân tích nguy cơ và hỗ trợ kết nối bác sĩ.</p>
  </div>

  <!-- INPUT -->
  <div id="input" class="card hidden">
    <h2>Nhập thông tin</h2>
    <input type="number" id="sugar" placeholder="Đường huyết (mg/dL)">
    <input type="number" id="pressure" placeholder="Huyết áp">

    <select id="symptom">
      <option value="none">Không triệu chứng</option>
      <option value="tired">Mệt</option>
      <option value="dizzy">Chóng mặt</option>
    </select>

    <select id="food">
      <option value="normal">Ăn bình thường</option>
      <option value="sweet">Ăn nhiều đồ ngọt</option>
    </select>

    <button onclick="analyze()">Phân tích</button>
    <p id="result"></p>
  </div>

  <!-- HISTORY -->
  <div id="history" class="card hidden">
    <h2>Lịch sử</h2>
    <ul id="historyList"></ul>
  </div>

  <!-- DOCTOR -->
  <div id="doctor" class="card hidden">
    <h2>Hỏi bác sĩ</h2>
    <textarea id="question" placeholder="Nhập câu hỏi..."></textarea>
    <button onclick="askDoctor()">Gửi</button>
    <p id="doctorReply"></p>
  </div>

</div>

<script>
function showPage(page) {
  document.querySelectorAll('.card').forEach(div => div.classList.add('hidden'));
  document.getElementById(page).classList.remove('hidden');
}

function analyze() {
  let sugar = document.getElementById('sugar').value;
  let food = document.getElementById('food').value;
  let symptom = document.getElementById('symptom').value;

  let result = "Ổn định";

  if (sugar > 140) result = "⚠️ Đường huyết cao";
  if (food === 'sweet') result = "⚠️ Ăn nhiều đồ ngọt";
  if (sugar > 140 && symptom !== 'none') result = "🚨 Nguy cơ cao, nên đi khám";

  document.getElementById('result').innerText = result;

  let data = JSON.parse(localStorage.getItem('history')) || [];
  data.push(`Đường: ${sugar} - ${result}`);
  localStorage.setItem('history', JSON.stringify(data));

  loadHistory();
}

function loadHistory() {
  let list = document.getElementById('historyList');
  list.innerHTML = "";
  let data = JSON.parse(localStorage.getItem('history')) || [];
  data.forEach(item => {
    let li = document.createElement('li');
    li.innerText = item;
    list.appendChild(li);
  });
}

function askDoctor() {
  let reply = "Bác sĩ khuyên bạn theo dõi thêm và giảm đồ ngọt.";
  document.getElementById('doctorReply').innerText = reply;
}

loadHistory();
</script>

</body>
</html>
