<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>루틴 달력</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root {
  --push: #E05C3A;
  --eng: #2A9D8F;
  --jpn: #7B61FF;
  --gold: #F4A623;
  --silver: #9BA4B5;
  --bronze: #CD7F4E;
  --bg: #F5F3EE;
  --surface: #FFFFFF;
  --text: #1A1A1A;
  --muted: #888;
  --border: rgba(0,0,0,0.08);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: 'Noto Sans KR', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  padding: 2rem;
}

.app { max-width: 860px; margin: 0 auto; }

.top-bar {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  margin-bottom: 1.5rem;
}

.app-title {
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--muted);
  margin-bottom: 4px;
}

.month-display {
  font-size: 34px;
  font-weight: 700;
  color: var(--text);
  line-height: 1;
  font-family: 'DM Mono', monospace;
}

.nav { display: flex; align-items: center; gap: 8px; padding-top: 8px; }

.nav-btn {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 50%;
  width: 36px; height: 36px;
  cursor: pointer; font-size: 18px;
  display: flex; align-items: center; justify-content: center;
  color: var(--text); transition: all 0.15s;
}
.nav-btn:hover { background: var(--text); color: var(--bg); }

.today-btn {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 0 14px; height: 36px;
  font-size: 12px; font-weight: 500;
  cursor: pointer; color: var(--text);
  font-family: 'Noto Sans KR', sans-serif;
  transition: all 0.15s;
}
.today-btn:hover { background: var(--text); color: var(--bg); }

/* Streak banner */
.streak-banner {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 14px 18px;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  flex-wrap: wrap;
}

.streak-main { display: flex; align-items: center; gap: 14px; }
.streak-fire { font-size: 28px; line-height: 1; }
.streak-count {
  font-size: 28px; font-weight: 700;
  font-family: 'DM Mono', monospace;
  line-height: 1; color: var(--gold);
}
.streak-sub { font-size: 11px; color: var(--muted); margin-top: 2px; }

.streak-milestones { display: flex; gap: 8px; }

.milestone {
  display: flex; flex-direction: column; align-items: center; gap: 3px;
  padding: 8px 12px;
  border-radius: 12px;
  border: 1px solid var(--border);
  background: var(--bg);
  min-width: 62px;
  position: relative;
  transition: all 0.2s;
}

.milestone.unlocked.m7   { background: rgba(205,127,78,0.08); border-color: rgba(205,127,78,0.4); }
.milestone.unlocked.m30  { background: rgba(155,164,181,0.1); border-color: rgba(155,164,181,0.4); }
.milestone.unlocked.m100 { background: rgba(244,166,35,0.1);  border-color: rgba(244,166,35,0.4); }
.milestone.next { border-style: dashed; border-color: rgba(0,0,0,0.18); }

.ms-check {
  position: absolute; top: -6px; right: -6px;
  width: 16px; height: 16px;
  border-radius: 50%; background: var(--gold);
  display: none; align-items: center; justify-content: center;
  font-size: 9px; color: white;
}
.milestone.unlocked .ms-check { display: flex; }

.ms-icon { font-size: 18px; }
.ms-label {
  font-size: 10px; font-weight: 500;
  font-family: 'DM Mono', monospace; color: var(--muted);
}
.milestone.unlocked.m7   .ms-label { color: var(--bronze); }
.milestone.unlocked.m30  .ms-label { color: var(--silver); }
.milestone.unlocked.m100 .ms-label { color: var(--gold); }
.ms-status { font-size: 9px; color: var(--muted); line-height: 1.2; }
.milestone.unlocked .ms-status { display: none; }

/* Stats */
.stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 1rem; }

.stat-card {
  background: var(--surface);
  border-radius: 14px; padding: 12px 14px;
  border: 1px solid var(--border);
  display: flex; align-items: center; gap: 10px;
}

.stat-icon {
  width: 34px; height: 34px; border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 17px; flex-shrink: 0;
}
.stat-icon.push { background: rgba(224,92,58,0.1); }
.stat-icon.eng  { background: rgba(42,157,143,0.1); }
.stat-icon.jpn  { background: rgba(123,97,255,0.1); }

.stat-num { font-size: 20px; font-weight: 700; font-family: 'DM Mono', monospace; line-height: 1; }
.stat-num.push { color: var(--push); }
.stat-num.eng  { color: var(--eng); }
.stat-num.jpn  { color: var(--jpn); }
.stat-label { font-size: 10px; color: var(--muted); margin-top: 2px; }

/* Legend */
.legend { display: flex; gap: 14px; margin-bottom: 1rem; flex-wrap: wrap; }
.leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--muted); }
.leg-dot { width: 7px; height: 7px; border-radius: 50%; }

/* Calendar */
.calendar {
  background: var(--surface);
  border-radius: 18px;
  border: 1px solid var(--border);
  overflow: hidden;
}

.dow-row {
  display: grid; grid-template-columns: repeat(7, 1fr);
  border-bottom: 1px solid var(--border);
}
.dow {
  text-align: center; font-size: 11px; font-weight: 500;
  letter-spacing: 0.07em; padding: 10px 0; color: var(--muted);
}
.dow.sun { color: var(--push); }
.dow.sat { color: #4A90D9; }

.days-grid { display: grid; grid-template-columns: repeat(7, 1fr); }

.day {
  border-right: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  padding: 7px; min-height: 96px; position: relative;
}
.day:nth-child(7n) { border-right: none; }
.day.empty { background: rgba(0,0,0,0.01); }

.day.streak-day { background: rgba(244,166,35,0.04); }
.day.streak-day::after {
  content: '';
  position: absolute; bottom: 0; left: 0; right: 0; height: 2px;
  background: var(--gold); opacity: 0.45;
}

.day-num-wrap { margin-bottom: 5px; display: inline-block; }
.day-num {
  font-size: 12px; font-weight: 500;
  font-family: 'DM Mono', monospace;
  color: var(--text); line-height: 1; display: inline-block;
}
.day-num.sun { color: var(--push); }
.day-num.sat { color: #4A90D9; }

.day.is-today .day-num-wrap {
  display: inline-flex; align-items: center; justify-content: center;
  width: 22px; height: 22px;
  background: var(--text); border-radius: 50%; margin-bottom: 3px;
}
.day.is-today .day-num { color: var(--bg) !important; }

.habits { display: flex; flex-direction: column; gap: 3px; }
.h-row { display: flex; align-items: center; gap: 4px; }

.h-check {
  width: 14px; height: 14px; border-radius: 4px; border: 1.5px solid;
  cursor: pointer; display: flex; align-items: center; justify-content: center;
  flex-shrink: 0; transition: all 0.1s; user-select: none;
}
.h-check svg { display: none; pointer-events: none; }
.h-check.done { border-color: transparent !important; }
.h-check.done svg { display: block; }
.h-check.push { border-color: var(--push); }
.h-check.push.done { background: var(--push); }
.h-check.eng  { border-color: var(--eng); }
.h-check.eng.done  { background: var(--eng); }
.h-check.jpn  { border-color: var(--jpn); }
.h-check.jpn.done  { background: var(--jpn); }
.h-label { font-size: 9px; color: var(--muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

.all-done-badge {
  position: absolute; top: 5px; right: 5px;
  width: 7px; height: 7px; border-radius: 50%;
  background: var(--gold); display: none;
}
.all-done-badge.show { display: block; }

.streak-num-cell {
  position: absolute; bottom: 4px; right: 5px;
  font-size: 8px; font-family: 'DM Mono', monospace;
  color: var(--gold); font-weight: 500; display: none;
}
.streak-num-cell.show { display: block; }

/* Progress */
.progress-wrap { margin-top: 14px; }
.progress-bar { height: 3px; background: rgba(0,0,0,0.06); border-radius: 2px; overflow: hidden; }
.progress-fill {
  height: 100%; border-radius: 2px;
  background: linear-gradient(90deg, var(--push), var(--jpn));
  transition: width 0.3s;
}
.progress-label {
  font-size: 11px; color: var(--muted); margin-top: 5px;
  text-align: right; font-family: 'DM Mono', monospace;
}

/* Modal */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.52);
  display: flex; align-items: center; justify-content: center;
  z-index: 1000; opacity: 0; pointer-events: none;
  transition: opacity 0.25s;
}
.modal-overlay.show { opacity: 1; pointer-events: all; }

.modal {
  background: var(--surface);
  border-radius: 24px; padding: 2.5rem 2rem 2rem;
  max-width: 360px; width: 90%; text-align: center;
  transform: scale(0.9) translateY(12px);
  transition: transform 0.32s cubic-bezier(0.34,1.56,0.64,1);
}
.modal-overlay.show .modal { transform: scale(1) translateY(0); }

.modal-emoji { font-size: 64px; line-height: 1; margin-bottom: 1rem; }
.modal-tag {
  font-size: 12px; font-weight: 500;
  letter-spacing: 0.1em; text-transform: uppercase;
  color: var(--muted); margin-bottom: 6px;
}
.modal-title { font-size: 24px; font-weight: 700; margin-bottom: 10px; line-height: 1.3; }
.modal-desc { font-size: 14px; color: var(--muted); line-height: 1.75; margin-bottom: 1.4rem; }

.modal-reward {
  background: var(--bg); border-radius: 14px; padding: 14px 16px;
  margin-bottom: 1.5rem; border: 1px dashed rgba(0,0,0,0.12);
}
.modal-reward-label {
  font-size: 10px; font-weight: 500; letter-spacing: 0.08em;
  text-transform: uppercase; color: var(--muted); margin-bottom: 5px;
}
.modal-reward-text { font-size: 14px; font-weight: 700; color: var(--text); }

.modal-close {
  background: var(--text); color: var(--bg);
  border: none; border-radius: 14px; padding: 12px 32px;
  font-size: 14px; font-weight: 500; cursor: pointer;
  font-family: 'Noto Sans KR', sans-serif; width: 100%;
  transition: opacity 0.15s;
}
.modal-close:hover { opacity: 0.82; }

/* Confetti */
.confetti-wrap {
  position: fixed; inset: 0;
  pointer-events: none; z-index: 999; overflow: hidden;
}
.cp {
  position: absolute; top: -12px; border-radius: 2px;
  animation: fall linear forwards;
}
@keyframes fall {
  0%   { transform: translateY(0) rotate(0deg); opacity: 1; }
  100% { transform: translateY(110vh) rotate(720deg); opacity: 0; }
}

@media (max-width: 600px) {
  body { padding: 1rem; }
  .month-display { font-size: 24px; }
  .stats { grid-template-columns: 1fr; }
  .day { min-height: 76px; padding: 5px; }
  .streak-milestones { gap: 5px; }
  .milestone { min-width: 52px; padding: 6px 8px; }
}
</style>
</head>
<body>
<div class="app">

  <div class="top-bar">
    <div>
      <div class="app-title">루틴 트래커</div>
      <div class="month-display" id="month-display"></div>
    </div>
    <div class="nav">
      <button class="nav-btn" id="prev">&#8249;</button>
      <button class="today-btn" id="go-today">오늘</button>
      <button class="nav-btn" id="next">&#8250;</button>
    </div>
  </div>

  <div class="streak-banner">
    <div class="streak-main">
      <div class="streak-fire" id="streak-fire">💤</div>
      <div>
        <div class="streak-count" id="streak-count">0</div>
        <div class="streak-sub" id="streak-sub">오늘 루틴을 시작해보세요</div>
      </div>
    </div>
    <div class="streak-milestones">
      <div class="milestone m7" id="m7">
        <div class="ms-check">✓</div>
        <div class="ms-icon">🥉</div>
        <div class="ms-label">7일</div>
        <div class="ms-status" id="m7-status">달성 전</div>
      </div>
      <div class="milestone m30" id="m30">
        <div class="ms-check">✓</div>
        <div class="ms-icon">🥈</div>
        <div class="ms-label">30일</div>
        <div class="ms-status" id="m30-status">달성 전</div>
      </div>
      <div class="milestone m100" id="m100">
        <div class="ms-check">✓</div>
        <div class="ms-icon">🏆</div>
        <div class="ms-label">100일</div>
        <div class="ms-status" id="m100-status">달성 전</div>
      </div>
    </div>
  </div>

  <div class="stats">
    <div class="stat-card">
      <div class="stat-icon push">💪</div>
      <div>
        <div class="stat-num push" id="s-push">0</div>
        <div class="stat-label">푸쉬업 50개 · 완료일</div>
      </div>
    </div>
    <div class="stat-card">
      <div class="stat-icon eng">📖</div>
      <div>
        <div class="stat-num eng" id="s-eng">0</div>
        <div class="stat-label">영어 5페이지 · 완료일</div>
      </div>
    </div>
    <div class="stat-card">
      <div class="stat-icon jpn">🇯🇵</div>
      <div>
        <div class="stat-num jpn" id="s-jpn">0</div>
        <div class="stat-label">일본어 20단어 · 완료일</div>
      </div>
    </div>
  </div>

  <div class="legend">
    <div class="leg"><div class="leg-dot" style="background:var(--push)"></div>푸쉬업 50개</div>
    <div class="leg"><div class="leg-dot" style="background:var(--eng)"></div>영어 5페이지</div>
    <div class="leg"><div class="leg-dot" style="background:var(--jpn)"></div>일본어 20단어</div>
    <div class="leg"><div class="leg-dot" style="background:var(--gold)"></div>3개 모두 완료</div>
    <div class="leg"><div class="leg-dot" style="background:var(--gold);opacity:0.35"></div>연속 달성</div>
  </div>

  <div class="calendar">
    <div class="dow-row">
      <div class="dow sun">일</div>
      <div class="dow">월</div>
      <div class="dow">화</div>
      <div class="dow">수</div>
      <div class="dow">목</div>
      <div class="dow">금</div>
      <div class="dow sat">토</div>
    </div>
    <div class="days-grid" id="days-grid"></div>
  </div>

  <div class="progress-wrap">
    <div class="progress-bar">
      <div class="progress-fill" id="progress-fill" style="width:0%"></div>
    </div>
    <div class="progress-label" id="progress-label">이번 달 진행률 0%</div>
  </div>

</div>

<!-- Modal -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal">
    <div class="modal-emoji" id="modal-emoji">🏆</div>
    <div class="modal-tag" id="modal-tag">연속 달성</div>
    <div class="modal-title" id="modal-title">축하해요!</div>
    <div class="modal-desc" id="modal-desc"></div>
    <div class="modal-reward">
      <div class="modal-reward-label">🎁 획득한 보상</div>
      <div class="modal-reward-text" id="modal-reward-text"></div>
    </div>
    <button class="modal-close" id="modal-close">계속하기 →</button>
  </div>
</div>

<div class="confetti-wrap" id="confetti-wrap"></div>

<script>
const HABITS = [
  { key: 'push', cls: 'push', label: '푸쉬업 50' },
  { key: 'eng',  cls: 'eng',  label: '영어 5p' },
  { key: 'jpn',  cls: 'jpn',  label: '단어 20' },
];

const MILESTONES = [
  {
    days: 7,
    id: 'm7',
    emoji: '🥉',
    tag: '7일 연속 달성',
    title: '첫 번째 목표 달성!',
    desc: '일주일을 꽉 채웠어요.\n꾸준함이 최고의 재능이라는 걸\n이미 증명하고 있어요.',
    reward: '브론즈 루틴 뱃지 🥉 획득',
    colors: ['#CD7F4E','#E89C6A','#F5C490','#FBE0C0'],
  },
  {
    days: 30,
    id: 'm30',
    emoji: '🥈',
    tag: '30일 연속 달성',
    title: '한 달 완주!',
    desc: '30일을 한 번도 빠지지 않았어요.\n이제 루틴이 완전히\n몸에 배기 시작했을 거예요.',
    reward: '실버 루틴 마스터 뱃지 🥈 획득',
    colors: ['#9BA4B5','#BCC4D4','#DCE4F4','#EEF4FF'],
  },
  {
    days: 100,
    id: 'm100',
    emoji: '🏆',
    tag: '100일 연속 달성',
    title: '루틴의 달인!',
    desc: '100일을 해냈어요!\n푸쉬업·영어·일본어가\n이제 삶의 일부가 되었어요.',
    reward: '황금 루틴 레전드 트로피 🏆 획득',
    colors: ['#F4A623','#FAC75A','#FDE39A','#FFF6D0'],
  },
];

function nowKST() {
  const d = new Date();
  return new Date(d.getTime() + d.getTimezoneOffset() * 60000 + 9 * 3600000);
}

const today = nowKST();
let cur = { y: today.getFullYear(), m: today.getMonth() };

function storeKey(y, m) { return `habit_v2_${y}_${m}`; }
function loadMonth(y, m) {
  try { return JSON.parse(localStorage.getItem(storeKey(y, m)) || '{}'); } catch(e) { return {}; }
}
function saveMonth(y, m, data) {
  try { localStorage.setItem(storeKey(y, m), JSON.stringify(data)); } catch(e) {}
}
function loadAcked() {
  try { return JSON.parse(localStorage.getItem('habit_acked') || '[]'); } catch(e) { return []; }
}
function saveAcked(arr) {
  try { localStorage.setItem('habit_acked', JSON.stringify(arr)); } catch(e) {}
}

function isDayDone(y, m, d) {
  const data = loadMonth(y, m);
  const day = data[d];
  return !!(day && day.push && day.eng && day.jpn);
}

function computeStreak() {
  let streak = 0;
  let y = today.getFullYear();
  let m = today.getMonth();
  let d = today.getDate();
  while (true) {
    if (d < 1) {
      m--;
      if (m < 0) { m = 11; y--; }
      d = new Date(y, m + 1, 0).getDate();
    }
    if (isDayDone(y, m, d)) { streak++; d--; }
    else break;
  }
  return streak;
}

function computeMonthStreakMap(y, m) {
  const days = new Date(y, m + 1, 0).getDate();
  const map = {};
  for (let d = 1; d <= days; d++) {
    if (!isDayDone(y, m, d)) continue;
    let s = 0;
    let wy = y, wm = m, wd = d;
    while (true) {
      if (wd < 1) {
        wm--; if (wm < 0) { wm = 11; wy--; }
        wd = new Date(wy, wm + 1, 0).getDate();
      }
      if (isDayDone(wy, wm, wd)) { s++; wd--; } else break;
    }
    map[d] = s;
  }
  return map;
}

function launchConfetti(colors) {
  const wrap = document.getElementById('confetti-wrap');
  wrap.innerHTML = '';
  for (let i = 0; i < 90; i++) {
    const p = document.createElement('div');
    p.className = 'cp';
    p.style.left = Math.random() * 100 + 'vw';
    p.style.background = colors[Math.floor(Math.random() * colors.length)];
    p.style.width  = (6 + Math.random() * 6) + 'px';
    p.style.height = (6 + Math.random() * 10) + 'px';
    p.style.animationDuration = (1.6 + Math.random() * 2) + 's';
    p.style.animationDelay   = (Math.random() * 0.8) + 's';
    wrap.appendChild(p);
  }
  setTimeout(() => { wrap.innerHTML = ''; }, 4500);
}

let pendingMs = [], msIdx = 0;

function showNextMs() {
  if (msIdx >= pendingMs.length) return;
  const ms = pendingMs[msIdx++];
  document.getElementById('modal-emoji').textContent  = ms.emoji;
  document.getElementById('modal-tag').textContent    = ms.tag;
  document.getElementById('modal-title').textContent  = ms.title;
  document.getElementById('modal-desc').textContent   = ms.desc;
  document.getElementById('modal-reward-text').textContent = ms.reward;
  document.getElementById('modal-overlay').classList.add('show');
  launchConfetti(ms.colors);

  document.getElementById('modal-close').onclick = () => {
    document.getElementById('modal-overlay').classList.remove('show');
    const acked = loadAcked();
    if (!acked.includes(ms.days)) { acked.push(ms.days); saveAcked(acked); }
    setTimeout(showNextMs, 400);
  };
}

function checkMilestones(streak) {
  const acked = loadAcked();
  pendingMs = MILESTONES.filter(ms => streak >= ms.days && !acked.includes(ms.days));
  msIdx = 0;
  if (pendingMs.length) setTimeout(showNextMs, 700);
}

document.getElementById('modal-overlay').addEventListener('click', (e) => {
  if (e.target === document.getElementById('modal-overlay'))
    document.getElementById('modal-close').click();
});

function updateBanner(streak) {
  document.getElementById('streak-count').textContent = streak;
  const fire = streak >= 100 ? '🔥🔥🔥' : streak >= 30 ? '🔥🔥' : streak > 0 ? '🔥' : '💤';
  document.getElementById('streak-fire').textContent = fire;
  document.getElementById('streak-sub').textContent  = streak > 0 ? '일 연속 달성 중!' : '오늘 루틴을 시작해보세요';

  MILESTONES.forEach((ms, i) => {
    const el = document.getElementById(ms.id);
    const st = document.getElementById(ms.id + '-status');
    el.classList.remove('unlocked', 'next');
    if (streak >= ms.days) {
      el.classList.add('unlocked');
    } else {
      if (st) st.textContent = (ms.days - streak) + '일 남음';
      const prev = MILESTONES[i - 1];
      if (!prev || streak >= prev.days) el.classList.add('next');
    }
  });
}

function render() {
  const months = ['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'];
  document.getElementById('month-display').textContent = `${cur.y}년 ${months[cur.m]}`;

  const streak = computeStreak();
  updateBanner(streak);
  const sMap = computeMonthStreakMap(cur.y, cur.m);
  const data = loadMonth(cur.y, cur.m);

  const grid = document.getElementById('days-grid');
  grid.innerHTML = '';

  const firstDow   = new Date(cur.y, cur.m, 1).getDay();
  const daysInMonth = new Date(cur.y, cur.m + 1, 0).getDate();
  let sPush = 0, sEng = 0, sJpn = 0, totalChecks = 0;

  for (let i = 0; i < firstDow; i++) {
    const e = document.createElement('div');
    e.className = 'day empty';
    grid.appendChild(e);
  }

  for (let d = 1; d <= daysInMonth; d++) {
    const dow = (firstDow + d - 1) % 7;
    const isToday = cur.y === today.getFullYear() && cur.m === today.getMonth() && d === today.getDate();
    const dayStreak = sMap[d] || 0;

    const cell = document.createElement('div');
    cell.className = 'day' + (isToday ? ' is-today' : '') + (dayStreak > 0 ? ' streak-day' : '');

    const nWrap = document.createElement('div');
    nWrap.className = 'day-num-wrap';
    const nEl = document.createElement('span');
    nEl.className = 'day-num' + (dow === 0 ? ' sun' : dow === 6 ? ' sat' : '');
    nEl.textContent = d;
    nWrap.appendChild(nEl);
    cell.appendChild(nWrap);

    const badge = document.createElement('div');
    badge.className = 'all-done-badge';
    cell.appendChild(badge);

    const sEl = document.createElement('div');
    sEl.className = 'streak-num-cell';
    if (dayStreak >= 3) { sEl.textContent = dayStreak + '🔥'; sEl.classList.add('show'); }
    cell.appendChild(sEl);

    const habDiv = document.createElement('div');
    habDiv.className = 'habits';
    let allDone = true;

    HABITS.forEach(h => {
      const done = !!(data[d] && data[d][h.key]);
      if (done) {
        if (h.key === 'push') sPush++;
        if (h.key === 'eng')  sEng++;
        if (h.key === 'jpn')  sJpn++;
        totalChecks++;
      } else { allDone = false; }

      const row = document.createElement('div');
      row.className = 'h-row';
      const chk = document.createElement('div');
      chk.className = `h-check ${h.cls}${done ? ' done' : ''}`;
      chk.innerHTML = `<svg width="8" height="8" viewBox="0 0 8 8"><polyline points="1.5,4 3,6 6.5,2" stroke="white" stroke-width="1.6" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>`;
      chk.addEventListener('click', () => {
        const d2 = loadMonth(cur.y, cur.m);
        if (!d2[d]) d2[d] = {};
        d2[d][h.key] = !d2[d][h.key];
        saveMonth(cur.y, cur.m, d2);
        render();
        checkMilestones(computeStreak());
      });
      const lbl = document.createElement('div');
      lbl.className = 'h-label';
      lbl.textContent = h.label;
      row.appendChild(chk); row.appendChild(lbl);
      habDiv.appendChild(row);
    });

    if (allDone) badge.classList.add('show');
    cell.appendChild(habDiv);
    grid.appendChild(cell);
  }

  document.getElementById('s-push').textContent = sPush;
  document.getElementById('s-eng').textContent  = sEng;
  document.getElementById('s-jpn').textContent  = sJpn;

  const pct = Math.round(totalChecks / (daysInMonth * 3) * 100);
  document.getElementById('progress-fill').style.width = pct + '%';
  document.getElementById('progress-label').textContent = `이번 달 진행률 ${pct}%`;
}

document.getElementById('prev').addEventListener('click', () => {
  cur.m--; if (cur.m < 0) { cur.m = 11; cur.y--; } render();
});
document.getElementById('next').addEventListener('click', () => {
  cur.m++; if (cur.m > 11) { cur.m = 0; cur.y++; } render();
});
document.getElementById('go-today').addEventListener('click', () => {
  cur = { y: today.getFullYear(), m: today.getMonth() }; render();
});

render();
</script>
</body>
</html>
