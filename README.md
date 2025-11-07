<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Mini Casino Demo — Coin Flip</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0f1724;
    --card:#0f1724;
    --panel:#0f1420aa;
    --accent:#6ee7b7;
    --muted:#94a3b8;
    --glass: rgba(255,255,255,0.04);
    --glass-2: rgba(255,255,255,0.02);
  }
  *{box-sizing:border-box}
  body{
    margin:0;
    font-family:Inter,system-ui,Arial;
    background: radial-gradient(900px 600px at 10% 10%, rgba(83,63,201,0.12), transparent),
                radial-gradient(800px 500px at 90% 90%, rgba(6,182,212,0.06), transparent),
                var(--bg);
    color:#e6eef8;
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
    min-height:100vh;
    display:flex;
    align-items:flex-start;
    justify-content:center;
    padding:36px;
  }

  .container{
    width:1100px;
    max-width:96%;
    display:grid;
    grid-template-columns: 420px 1fr;
    gap:20px;
    align-items:start;
  }

  /* left panel */
  .panel{
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    border:1px solid rgba(255,255,255,0.04);
    border-radius:14px;
    padding:20px;
    backdrop-filter: blur(8px);
    box-shadow: 0 10px 30px rgba(2,6,23,0.6);
  }

  .brand{
    display:flex;
    gap:12px;
    align-items:center;
    margin-bottom:14px;
  }
  .logo{
    width:52px;height:52px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#60a5fa);display:flex;
    align-items:center;justify-content:center;font-weight:700;color:#06202a;font-size:20px;
    box-shadow: 0 6px 18px rgba(99,102,241,0.12), inset 0 -6px 12px rgba(255,255,255,0.04);
  }
  h1{font-size:18px;margin:0}
  p.lead{margin:6px 0 18px;color:var(--muted);font-size:13px}

  .balance{
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:14px;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);
    border-radius:10px;margin-bottom:14px;border:1px solid rgba(255,255,255,0.02);
  }
  .bal-left{display:flex;gap:12px;align-items:center}
  .bal-amount{font-size:20px;font-weight:600}
  .bal-currency{color:var(--muted);font-size:13px}
  .small-btn{
    background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 12px;border-radius:10px;color:var(--muted);
    cursor:pointer;font-size:13px
  }

  /* game box */
  .game{
    margin-top:10px;
    padding:16px;border-radius:12px;background:linear-gradient(180deg, rgba(255,255,255,0.015), transparent);
    border:1px solid rgba(255,255,255,0.02);
  }
  .row{display:flex;gap:12px;align-items:center}
  .input{
    background:var(--glass);border-radius:10px;padding:10px;border:1px solid rgba(255,255,255,0.02);flex:1;
  }
  label{font-size:13px;color:var(--muted);display:block;margin-bottom:6px}

  .bet-controls{display:flex;gap:10px;align-items:center;margin:12px 0}
  .slider{flex:1}
  input[type=range]{width:100%}
  .big-btn{
    background:linear-gradient(90deg,var(--accent),#60a5fa);color:#06202a;padding:12px 18px;border-radius:12px;border:none;
    font-weight:700;cursor:pointer;box-shadow:0 8px 18px rgba(55,125,255,0.12);
  }
  .ghost-btn{background:transparent;border:1px solid rgba(255,255,255,0.04);padding:10px 12px;border-radius:10px;color:var(--muted);cursor:pointer}

  .coin{
    width:120px;height:120px;border-radius:50%;background:linear-gradient(180deg,#fff2,#0002);display:flex;align-items:center;justify-content:center;
    margin:14px auto;border:6px solid rgba(255,255,255,0.04);font-weight:800;font-size:22px;position:relative;
    transition: transform 0.9s cubic-bezier(.17,.67,.49,1.01);
  }
  .coin.win{box-shadow:0 12px 40px rgba(110,231,183,0.16)}
  .coin.flip{transform:rotateY(720deg) rotateX(360deg) scale(1.03)}

  .msg{margin-top:10px;color:var(--muted);font-size:13px;min-height:20px}

  /* right panel - players table */
  .players{
    padding:18px;border-radius:14px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);
    border:1px solid rgba(255,255,255,0.02);
  }
  .players h2{margin:0 0 10px 0;font-size:16px}
  table{width:100%;border-collapse:collapse;font-size:14px}
  th,td{padding:10px 8px;text-align:left;border-bottom:1px solid rgba(255,255,255,0.02)}
  th{font-size:12px;color:var(--muted);font-weight:600}
  .player-row{display:flex;align-items:center;gap:12px}
  .avatar{width:36px;height:36px;border-radius:9px;background:linear-gradient(180deg,#fff2,#0002);display:flex;align-items:center;justify-content:center;font-weight:700;color:#06202a}
  .small{font-size:12px;color:var(--muted)}
  .win-pill{background:rgba(110,231,183,0.12);color:var(--accent);padding:6px 8px;border-radius:999px;font-weight:700;font-size:13px}

  footer.note{margin-top:12px;color:var(--muted);font-size:12px}

  /* responsive */
  @media (max-width:880px){
    .container{grid-template-columns:1fr;align-items:stretch}
    .logo{width:48px;height:48px}
  }

</style>
</head>
<body>
  <div class="container">
    <div class="panel">
      <div class="brand">
        <div class="logo">MC</div>
        <div>
          <h1>Mini Casino — демо</h1>
          <p class="lead">Игра: подбрасывание монетки. Меняйте ставку и играйте сразу. Это демонстрация — реальные платежи не подключены.</p>
        </div>
      </div>

      <div class="balance" aria-live="polite">
        <div class="bal-left">
          <div>
            <div class="bal-amount" id="balance">50 000 ₽</div>
            <div class="bal-currency">Баланс</div>
          </div>
        </div>
        <div>
          <button class="small-btn" id="guestBtn">Играть как гость</button>
        </div>
      </div>

      <div class="game">
        <label for="bet">Ставка</label>
        <div class="bet-controls">
          <input id="bet" type="number" class="input" min="1" step="100" value="1000" />
          <div style="width:110px;text-align:right">
            <div class="small">Шанс 50% — выигрышь x2</div>
            <div style="font-weight:700;margin-top:6px" id="potential">Потенциально: 2 000 ₽</div>
          </div>
        </div>

        <label>Настройка ставки (сдвиньте):</label>
        <input id="range" type="range" min="1" max="100000" value="1000" class="slider" />

        <div style="display:flex;gap:10px;align-items:center;margin-top:12px;justify-content:center;flex-direction:column">
          <div class="coin" id="coin">?</div>
          <div style="display:flex;gap:10px">
            <button class="big-btn" id="playBtn">Подбросить монетку</button>
            <button class="ghost-btn" id="doubleBtn">Поставить всё</button>
            <button class="ghost-btn" id="halfBtn">Половина</button>
          </div>
          <div class="msg" id="msg">Удачи! Максимальная демонстрационная ставка: 100 000 ₽</div>
        </div>

        <div style="margin-top:14px;display:flex;gap:12px;justify-content:space-between;align-items:center">
          <div>
            <button class="ghost-btn" id="depositBtn">Пополнить (демо +100k)</button>
            <button class="ghost-btn" id="withdrawBtn">Вывести</button>
          </div>
          <div class="small">Минимум для вывода: <strong id="minWithdraw">100 000 ₽</strong></div>
        </div>

        <footer class="note">Это демо. Для реальных выплат необходима лицензия и интеграция с платёжными провайдерами.</footer>
      </div>
    </div>

    <div class="players">
      <h2>Таблица игроков</h2>
      <table id="playersTable" aria-live="polite">
        <thead>
          <tr><th>Игрок</th><th>Баланс</th><th>Ставка</th><th>Результат</th></tr>
        </thead>
        <tbody>
          <!-- rows injected by JS -->
        </tbody>
      </table>
      <p class="small" style="margin-top:10px">Последние ходы отображаются в таблице. Демо-данные генерируются локально.</p>
    </div>
  </div>

<script>
/*
  Mini Casino Demo — frontend only
  - Симулирует игру "монетка"
  - Меняет баланс, обновляет таблицу игроков
  - НЕ содержит реальной платёжной логики
*/

const format = n => n.toLocaleString('ru-RU') + ' ₽';

let state = {
  balance: 50000,
  minWithdraw: 100000,
  players: [
    {name:'Алиса', balance:120000, bet:5000, result:'win'},
    {name:'Борис', balance:54000, bet:2000, result:'lose'},
    {name:'Катя', balance:83000, bet:1500, result:'win'},
  ]
};

const balanceEl = document.getElementById('balance');
const betInput = document.getElementById('bet');
const range = document.getElementById('range');
const potential = document.getElementById('potential');
const coin = document.getElementById('coin');
const msg = document.getElementById('msg');
const playersTableBody = document.querySelector('#playersTable tbody');
const minWithdrawEl = document.getElementById('minWithdraw');

function renderBalance(){
  balanceEl.textContent = format(state.balance);
}
function renderPotential(){
  const bet = Math.max(1, Number(betInput.value || 0));
  potential.textContent = 'Потенциально: ' + format(bet*2);
}
function syncRangeAndInput(){
  let v = Number(range.value);
  betInput.value = v;
  renderPotential();
}
range.addEventListener('input', () => {
  syncRangeAndInput();
});
betInput.addEventListener('change', () => {
  let v = Number(betInput.value || 0);
  if (v < 1) v = 1;
  if (v > Number(range.max)) {
    v = Number(range.max);
  }
  betInput.value = Math.round(v);
  range.value = betInput.value;
  renderPotential();
});

function renderPlayers(){
  playersTableBody.innerHTML = '';
  // latest first: include demo "you" as top
  const rows = [...state.players].slice().reverse();
  rows.forEach(p => {
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td>
        <div class="player-row">
          <div class="avatar">${p.name[0] || '?'}</div>
          <div>
            <div style="font-weight:700">${p.name}</div>
            <div class="small">Последний ход: ${p.result}</div>
          </div>
        </div>
      </td>
      <td>${format(p.balance)}</td>
      <td>${format(p.bet)}</td>
      <td>${p.result === 'win' ? '<span class="win-pill">WIN</span>' : '<span class="small">LOSE</span>'}</td>
    `;
    playersTableBody.appendChild(tr);
  });
}

function addToPlayers(name, balance, bet, result){
  state.players.push({name,balance,bet,result});
  if(state.players.length > 50) state.players.shift();
  renderPlayers();
}

function randomName(){
  const names = ['Дима','Елена','Игорь','Мария','Петя','Оля','Сергей','Лена','Коля','Валя'];
  return names[Math.floor(Math.random()*names.length)];
}

function playOnce(){
  const bet = Math.round(Number(betInput.value) || 0);
  if (bet <= 0){
    msg.textContent = 'Укажите ставку > 0';
    return;
  }
  if (bet > state.balance){
    msg.textContent = 'Недостаточно средств для ставки';
    return;
  }
  // start flip animation
  coin.classList.remove('win');
  coin.classList.add('flip');
  coin.textContent = '...';
  msg.textContent = 'Крутим монетку...';

  // simulate network latency
  setTimeout(() => {
    coin.classList.remove('flip');
    // 50/50
    const win = Math.random() < 0.5;
    if (win){
      const prize
