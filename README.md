<index.html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Пойдём гулять?</title>
<style>
  * { box-sizing: border-box; }
  body {
    font-family: Arial, sans-serif;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    margin: 0;
    background: linear-gradient(135deg, #fce4ec, #f8bbd0);
    padding: 20px;
  }
  .card {
    background: white;
    padding: 32px 28px;
    border-radius: 20px;
    box-shadow: 0 8px 30px rgba(0,0,0,0.12);
    text-align: center;
    max-width: 400px;
    width: 100%;
    position: relative;
  }
  h1 { margin: 0 0 8px; font-size: 26px; }
  p.sub { margin: 0 0 24px; color: #777; font-size: 15px; }
  button {
    padding: 14px 28px;
    font-size: 17px;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    margin: 6px;
    transition: transform 0.15s ease;
  }
  #yesBtn { background: #e91e63; color: white; }
  #yesBtn:hover { transform: scale(1.05); }
  #noBtn { background: #eee; color: #333; }
  #timeStep { display: none; }
  #timeStep h2 { margin: 0 0 20px; font-size: 20px; }
  label {
    display: block;
    text-align: left;
    font-size: 14px;
    color: #555;
    margin: 12px 0 4px;
  }
  input[type="date"], input[type="time"], select {
    width: 100%;
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 12px;
    outline: none;
  }
  input[type="date"]:focus, input[type="time"]:focus, select:focus {
    border-color: #e91e63;
  }
  #confirmBtn {
    background: #e91e63;
    color: white;
    width: 100%;
    margin-top: 20px;
    padding: 16px;
    font-size: 17px;
  }
  #result { display: none; }
  #result h2 { color: #e91e63; margin-top: 0; }
  #result p { font-size: 16px; line-height: 1.5; }
  .emoji { font-size: 48px; display: block; margin-bottom: 12px; }
</style>
</head>
<body>

<div class="card">
  <!-- Шаг 1: вопрос -->
  <div id="questionStep">
    <h1>Пойдём гулять?</h1>
    <p class="sub">Выбери вариант 😊</p>
    <button id="yesBtn">Да</button>
    <button id="noBtn">Нет</button>
  </div>

  <!-- Шаг 2: выбор времени -->
  <div id="timeStep">
    <h2>Когда удобно?</h2>
    <label for="date">Дата</label>
    <input type="date" id="date">
    <label for="time">Время</label>
    <input type="time" id="time">
    <label for="place">Место (необязательно)</label>
    <select id="place">
      <option value="">— выбери —</option>
      <option>Парк</option>
      <option>Кофейня</option>
      <option>Набережная</option>
      <option>Кино</option>
      <option>Другое</option>
    </select>
    <button id="confirmBtn">Подтвердить</button>
  </div>

  <!-- Шаг 3: итог -->
  <div id="result">
    <span class="emoji">🎉</span>
    <h2>Отлично!</h2>
    <p id="resultText"></p>
    <p style="color:#999; font-size:14px; margin-top:20px;">Скинь скрин этой страницы — и я подтвержу.</p>
  </div>
</div>

<script>
  const noBtn = document.getElementById('noBtn');
  const yesBtn = document.getElementById('yesBtn');
  const questionStep = document.getElementById('questionStep');
  const timeStep = document.getElementById('timeStep');
  const result = document.getElementById('result');
  const confirmBtn = document.getElementById('confirmBtn');
  const dateInput = document.getElementById('date');
  const timeInput = document.getElementById('time');
  const placeInput = document.getElementById('place');
  const resultText = document.getElementById('resultText');

  // Устанавливаем минимальную дату — сегодня
  const today = new Date().toISOString().split('T')[0];
  dateInput.min = today;

  // Кнопка "Нет" убегает
  function runAway() {
    const maxX = window.innerWidth - noBtn.offsetWidth - 20;
    const maxY = window.innerHeight - noBtn.offsetHeight - 20;
    noBtn.style.position = 'fixed';
    noBtn.style.left = Math.max(10, Math.random() * maxX) + 'px';
    noBtn.style.top = Math.max(10, Math.random() * maxY) + 'px';
  }
  noBtn.addEventListener('mouseover', runAway);
  noBtn.addEventListener('touchstart', (e) => { e.preventDefault(); runAway(); });

  // Кнопка "Да"
  yesBtn.addEventListener('click', () => {
    questionStep.style.display = 'none';
    timeStep.style.display = 'block';
  });

  // Подтверждение
  confirmBtn.addEventListener('click', () => {
    if (!dateInput.value || !timeInput.value) {
      alert('Выбери дату и время, пожалуйста 🙂');
      return;
    }
    const d = new Date(dateInput.value + 'T' + timeInput.value);
    const options = { day: 'numeric', month: 'long', year: 'numeric', hour: '2-digit', minute: '2-digit' };
    const formatted = d.toLocaleString('ru-RU', options);

    const place = placeInput.value ? ` Место: ${placeInput.value}.` : '';
    resultText.textContent = `Договорились: ${formatted}.${place} Жду!`;

    timeStep.style.display = 'none';
    result.style.display = 'block';
  });
</script>

</body>
</html>