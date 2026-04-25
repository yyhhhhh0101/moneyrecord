# moneyrecord
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="暄祥記帳">
<title>暄 & 祥 記帳本</title>
<style>
  :root {
    --bg: #ffffff;
    --bg2: #f5f5f3;
    --bg3: #eeede9;
    --text: #1a1a1a;
    --text2: #666660;
    --text3: #999993;
    --border: rgba(0,0,0,0.12);
    --border2: rgba(0,0,0,0.22);
    --green: #0F6E56;
    --green-bg: #E1F5EE;
    --blue: #185FA5;
    --blue-bg: #E6F1FB;
    --amber: #854F0B;
    --amber-bg: #FAEEDA;
    --red: #993C1D;
    --red-bg: #FAECE7;
    --radius: 12px;
    --radius-sm: 8px;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #1c1c1e;
      --bg2: #2c2c2e;
      --bg3: #3a3a3c;
      --text: #f5f5f0;
      --text2: #aeaea8;
      --text3: #6e6e68;
      --border: rgba(255,255,255,0.12);
      --border2: rgba(255,255,255,0.22);
      --green: #5DCAA5;
      --green-bg: rgba(15,110,86,0.2);
      --blue: #85B7EB;
      --blue-bg: rgba(24,95,165,0.2);
      --amber: #FAC775;
      --amber-bg: rgba(133,79,11,0.2);
      --red: #F0997B;
      --red-bg: rgba(153,60,29,0.2);
    }
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body { font-family: -apple-system, 'PingFang TC', sans-serif; background: var(--bg2); color: var(--text); min-height: 100vh; }

  /* Header */
  .header { background: var(--bg); border-bottom: 0.5px solid var(--border); padding: 14px 16px 0; position: sticky; top: 0; z-index: 50; }
  .header-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; }
  .header-title { font-size: 18px; font-weight: 600; }
  .header-sub { font-size: 12px; color: var(--text3); }
  .add-btn { background: var(--text); color: var(--bg); border: none; border-radius: var(--radius-sm); padding: 8px 14px; font-size: 13px; font-weight: 500; cursor: pointer; }

  /* Tabs */
  .tabs { display: flex; overflow-x: auto; scrollbar-width: none; gap: 0; }
  .tabs::-webkit-scrollbar { display: none; }
  .tab { flex-shrink: 0; padding: 10px 16px; font-size: 13px; color: var(--text2); border: none; background: none; cursor: pointer; border-bottom: 2px solid transparent; font-family: inherit; white-space: nowrap; }
  .tab.active { color: var(--text); border-bottom-color: var(--text); font-weight: 500; }

  /* Content */
  .content { padding: 16px; max-width: 600px; margin: 0 auto; }
  .section { display: none; }
  .section.active { display: block; }

  /* Cards */
  .card { background: var(--bg); border-radius: var(--radius); padding: 14px 16px; margin-bottom: 10px; border: 0.5px solid var(--border); }
  .card-title { font-size: 14px; font-weight: 600; margin-bottom: 10px; }

  /* Buttons */
  button { font-family: inherit; cursor: pointer; }
  .btn { padding: 8px 14px; border-radius: var(--radius-sm); border: 0.5px solid var(--border2); background: transparent; color: var(--text); font-size: 13px; font-family: inherit; }
  .btn:active { opacity: 0.7; }
  .btn-primary { background: var(--text); color: var(--bg); border-color: var(--text); font-weight: 500; }
  .btn-gmail { background: #D85A30; color: #fff; border-color: #D85A30; font-weight: 500; }
  .btn-danger { color: var(--red); border-color: transparent; background: transparent; padding: 4px 8px; font-size: 12px; }
  .btn-full { width: 100%; padding: 12px; display: flex; align-items: center; justify-content: center; gap: 8px; font-size: 14px; }

  /* Form elements */
  input, select { font-family: inherit; font-size: 14px; padding: 10px 12px; border: 0.5px solid var(--border2); border-radius: var(--radius-sm); background: var(--bg2); color: var(--text); width: 100%; appearance: none; -webkit-appearance: none; }
  input:focus, select:focus { outline: none; border-color: var(--text); }
  .form-group { margin-bottom: 14px; }
  .form-group > label { display: block; font-size: 12px; color: var(--text2); margin-bottom: 6px; font-weight: 500; text-transform: uppercase; letter-spacing: 0.04em; }

  /* Person toggle */
  .person-toggle { display: flex; gap: 6px; flex-wrap: wrap; }
  .ptog { padding: 6px 14px; font-size: 13px; border-radius: 99px; cursor: pointer; border: 0.5px solid var(--border); background: transparent; color: var(--text2); font-family: inherit; }
  .ptog.sel-xuan { background: var(--green-bg); color: var(--green); border-color: var(--green); }
  .ptog.sel-xiang { background: var(--blue-bg); color: var(--blue); border-color: var(--blue); }
  .ptog.sel-split { background: var(--amber-bg); color: var(--amber); border-color: var(--amber); }

  /* Badges / Pills */
  .pill { display: inline-block; font-size: 11px; padding: 2px 8px; border-radius: 99px; font-weight: 500; }
  .pill-ub { background: var(--red-bg); color: var(--red); }
  .pill-fp { background: rgba(153,53,86,0.12); color: #993556; }
  .pill-manual { background: var(--bg3); color: var(--text2); }
  .badge-fee { display: inline-block; font-size: 10px; padding: 1px 5px; border-radius: 4px; background: var(--bg3); color: var(--text2); margin-left: 4px; }

  /* Notice */
  .notice { background: var(--bg3); border-radius: var(--radius-sm); padding: 10px 14px; font-size: 13px; color: var(--text2); margin-bottom: 12px; line-height: 1.5; }

  /* Metrics */
  .metric-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 12px; }
  .metric { background: var(--bg2); border-radius: var(--radius-sm); padding: 12px; }
  .metric-label { font-size: 11px; color: var(--text3); margin-bottom: 4px; text-transform: uppercase; letter-spacing: 0.04em; }
  .metric-value { font-size: 22px; font-weight: 600; }

  /* Split item row */
  .split-item { padding: 12px 0; border-bottom: 0.5px solid var(--border); }
  .split-item:last-child { border-bottom: none; }
  .split-item-top { display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px; }
  .split-item-name { font-size: 14px; font-weight: 500; }
  .split-item-amt { font-size: 14px; color: var(--text2); }
  .pct-row { display: flex; align-items: center; gap: 8px; margin-top: 8px; }
  .pct-label { font-size: 12px; color: var(--text2); min-width: 70px; }
  input[type=range] { -webkit-appearance: none; appearance: none; height: 3px; background: var(--border2); border-radius: 99px; flex: 1; border: none; padding: 0; }
  input[type=range]::-webkit-slider-thumb { -webkit-appearance: none; width: 18px; height: 18px; border-radius: 50%; background: var(--text); }

  /* Ledger */
  .ledger-entry { display: grid; grid-template-columns: 1fr 60px 60px 60px 30px; gap: 4px; align-items: center; font-size: 13px; padding: 8px 0; border-bottom: 0.5px solid var(--border); }
  .ledger-entry.hdr { font-size: 11px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.03em; }
  .ledger-totals { display: flex; gap: 12px; padding-top: 8px; font-size: 13px; font-weight: 600; border-top: 0.5px solid var(--border); margin-top: 4px; }

  /* Empty */
  .empty { text-align: center; padding: 3rem 1rem; color: var(--text3); font-size: 14px; }

  /* Modal */
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 200; align-items: flex-end; justify-content: center; }
  .modal-overlay.open { display: flex; }
  .modal { background: var(--bg); border-radius: 20px 20px 0 0; padding: 20px 20px 40px; width: 100%; max-width: 600px; max-height: 90vh; overflow-y: auto; }
  .modal-handle { width: 36px; height: 4px; background: var(--border2); border-radius: 99px; margin: 0 auto 16px; }
  .modal-title { font-size: 17px; font-weight: 600; margin-bottom: 16px; }

  /* Success / Info banners */
  .banner-success { background: var(--green-bg); color: var(--green); padding: 10px 14px; border-radius: var(--radius-sm); font-size: 13px; margin-top: 8px; }
  .banner-info { background: var(--blue-bg); color: var(--blue); padding: 10px 14px; border-radius: var(--radius-sm); font-size: 13px; margin-top: 8px; }
  .banner-warn { background: var(--amber-bg); color: var(--amber); padding: 10px 14px; border-radius: var(--radius-sm); font-size: 13px; margin-top: 8px; }

  /* Loading dots */
  .dots span { display: inline-block; width: 5px; height: 5px; border-radius: 50%; background: currentColor; margin: 0 2px; animation: blink 1s infinite; }
  .dots span:nth-child(2) { animation-delay: .2s; }
  .dots span:nth-child(3) { animation-delay: .4s; }
  @keyframes blink { 0%,100%{opacity:.2} 50%{opacity:1} }

  .divider { height: 0.5px; background: var(--border); margin: 12px 0; }
</style>
</head>
<body>

<div class="header">
  <div class="header-top">
    <div>
      <div class="header-title">暄 &amp; 祥 記帳本</div>
      <div class="header-sub" id="today-label"></div>
    </div>
    <button class="add-btn" onclick="openModal()">+ 新增</button>
  </div>
  <div class="tabs">
    <button class="tab active" onclick="switchTab('order')">訂單記帳</button>
    <button class="tab" onclick="switchTab('ledger')">記帳總表</button>
    <button class="tab" onclick="switchTab('monthly')">月結算</button>
    <button class="tab" onclick="switchTab('settings')">設定</button>
  </div>
</div>

<div class="content">

  <!-- 訂單記帳 -->
  <div class="section active" id="tab-order">
    <div class="notice">下單後點下方按鈕抓取 UberEats 訂單，為每個品項選擇費用負擔方式，再存入記帳表。Foodpanda 請點「+ 新增」手動輸入。</div>
    <button class="btn btn-full" id="fetch-btn" onclick="fetchUberEats()" style="margin-bottom:12px;">↓ 從 Uber Eats 抓取最新訂單</button>

    <div id="order-items-wrap">
      <div class="empty">尚無訂單品項</div>
    </div>

    <div id="order-summary" style="display:none;" class="card">
      <div class="card-title">分攤預覽</div>
      <div class="metric-grid">
        <div class="metric"><div class="metric-label">暄 負擔</div><div class="metric-value" id="xuan-total" style="color:var(--green)">NT$0</div></div>
        <div class="metric"><div class="metric-label">祥 負擔</div><div class="metric-value" id="xiang-total" style="color:var(--blue)">NT$0</div></div>
      </div>
      <div style="display:flex;gap:8px;justify-content:flex-end;">
        <button class="btn" onclick="clearOrder()">清除</button>
        <button class="btn btn-primary" onclick="saveOrder()">存入記帳表</button>
      </div>
    </div>
  </div>

  <!-- 記帳總表 -->
  <div class="section" id="tab-ledger">
    <div id="ledger-wrap"><div class="empty">尚無記帳紀錄</div></div>
  </div>

  <!-- 月結算 -->
  <div class="section" id="tab-monthly">
    <div class="card">
      <div class="card-title">月結算</div>
      <div class="form-group">
        <label>選擇月份</label>
        <input type="month" id="month-picker">
      </div>
      <button class="btn btn-primary btn-full" onclick="calcMonthly()" style="margin-bottom:0;">計算</button>
      <div id="monthly-result" style="margin-top:14px;"></div>
    </div>
  </div>

  <!-- 設定 -->
  <div class="section" id="tab-settings">
    <div class="card">
      <div class="card-title">每日記帳提醒</div>
      <p style="font-size:13px;color:var(--text2);margin-bottom:10px;">每天晚上 10 點提醒記帳（需允許瀏覽器通知）</p>
      <button class="btn btn-full" onclick="enableReminder()" id="reminder-btn">開啟提醒通知</button>
      <div id="reminder-status"></div>
    </div>
    <div class="card">
      <div class="card-title">付款規則</div>
      <div style="font-size:13px;color:var(--text2);line-height:2;">
        UberEats → <strong style="color:var(--green)">暄</strong> 先付款<br>
        Foodpanda → <strong style="color:var(--blue)">祥</strong> 先付款
      </div>
    </div>
    <div class="card">
      <div class="card-title">月結算寄信</div>
      <p style="font-size:13px;color:var(--text2);">月結算信件寄送至：<br><strong>yihsuan890101@gmail.com</strong></p>
      <div class="banner-info" style="margin-top:10px;">在月結算頁面計算後，點「寄送月結算信」即可透過 Gmail 自動寄出。</div>
    </div>
    <div class="card">
      <div class="card-title">清除所有資料</div>
      <p style="font-size:13px;color:var(--text2);margin-bottom:10px;">清除本機所有記帳紀錄（不可復原）</p>
      <button class="btn btn-full" style="color:var(--red);border-color:var(--red);" onclick="clearAll()">清除所有紀錄</button>
    </div>
  </div>

</div><!-- /content -->

<!-- 新增品項 Modal -->
<div class="modal-overlay" id="add-modal" onclick="handleOverlayClick(event)">
  <div class="modal" id="modal-inner">
    <div class="modal-handle"></div>
    <div class="modal-title">新增品項</div>
    <div class="form-group">
      <label>平台</label>
      <select id="m-platform">
        <option value="UberEats">UberEats（暄先付）</option>
        <option value="Foodpanda">Foodpanda（祥先付）</option>
        <option value="手動">手動輸入</option>
      </select>
    </div>
    <div class="form-group">
      <label>品項名稱</label>
      <input id="m-name" placeholder="例：雞腿便當、外送費">
    </div>
    <div class="form-group">
      <label>金額（NT$）</label>
      <input id="m-amount" type="number" inputmode="decimal" placeholder="0" min="0">
    </div>
    <div class="form-group">
      <label>費用負擔</label>
      <div class="person-toggle">
        <button class="ptog sel-xuan" id="mtog-xuan" onclick="setMTog('xuan')">暄</button>
        <button class="ptog" id="mtog-xiang" onclick="setMTog('xiang')">祥</button>
        <button class="ptog" id="mtog-split" onclick="setMTog('split')">各半</button>
      </div>
    </div>
    <div id="split-detail" style="display:none;" class="form-group">
      <label>暄的比例（%）</label>
      <input id="m-xuan-pct" type="number" inputmode="decimal" value="50" min="0" max="100">
    </div>
    <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:16px;">
      <button class="btn" onclick="closeModal()">取消</button>
      <button class="btn btn-primary" onclick="addManualItem()">新增</button>
    </div>
  </div>
</div>

<script>
const KEY = 'xuan_xiang_v1';
let state = (() => { try { return JSON.parse(localStorage.getItem(KEY) || '{"entries":[],"pendingItems":[]}'); } catch(e) { return {entries:[],pendingItems:[]}; } })();
let mTog = 'xuan';

function save() { try { localStorage.setItem(KEY, JSON.stringify(state)); } catch(e) {} }

function today() { const d = new Date(); return `${d.getMonth()+1}/${d.getDate()}`; }
function todayKey() { const d = new Date(); return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`; }
function todayLabel() { const d = new Date(); const days = ['日','一','二','三','四','五','六']; return `${today()} 星期${days[d.getDay()]}`; }

document.getElementById('today-label').textContent = todayLabel();
const mp = document.getElementById('month-picker');
const now = new Date();
mp.value = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}`;

function switchTab(id) {
  document.querySelectorAll('.tab').forEach((t,i) => { t.classList.toggle('active', ['order','ledger','monthly','settings'][i]===id); });
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.getElementById('tab-'+id).classList.add('active');
  if(id==='ledger') renderLedger();
}

function openModal() { document.getElementById('add-modal').classList.add('open'); }
function closeModal() { document.getElementById('add-modal').classList.remove('open'); }
function handleOverlayClick(e) { if(e.target === document.getElementById('add-modal')) closeModal(); }

function setMTog(v) {
  mTog = v;
  ['xuan','xiang','split'].forEach(k => { document.getElementById('mtog-'+k).className = 'ptog' + (v===k?' sel-'+k:''); });
  document.getElementById('split-detail').style.display = v==='split'?'block':'none';
}

function computeAmounts(amount, bearer, xuanPct=50) {
  if(bearer==='xuan') return {xuanAmt: amount, xiangAmt: 0};
  if(bearer==='xiang') return {xuanAmt: 0, xiangAmt: amount};
  const xuanAmt = Math.round(amount * xuanPct / 100);
  return {xuanAmt, xiangAmt: amount - xuanAmt};
}

function addManualItem() {
  const name = document.getElementById('m-name').value.trim();
  const amt = parseFloat(document.getElementById('m-amount').value) || 0;
  const platform = document.getElementById('m-platform').value;
  if(!name || !amt) { alert('請填寫品項名稱和金額'); return; }
  const xuanPct = parseFloat(document.getElementById('m-xuan-pct').value) || 50;
  const {xuanAmt, xiangAmt} = computeAmounts(amt, mTog, xuanPct);
  const payer = platform==='Foodpanda' ? '祥' : '暄';
  state.entries.push({ id: Date.now()+Math.random(), date: todayKey(), label: `${today()} ${platform}`, platform, payer, name, amount: amt, xuanAmt, xiangAmt, bearer: mTog });
  save();
  closeModal();
  document.getElementById('m-name').value = '';
  document.getElementById('m-amount').value = '';
}

async function fetchUberEats() {
  const btn = document.getElementById('fetch-btn');
  btn.disabled = true;
  btn.innerHTML = '<span class="dots"><span></span><span></span><span></span></span> 連接 Uber Eats 中...';
  try {
    const resp = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST', headers:{'Content-Type':'application/json'},
      body: JSON.stringify({
        model:'claude-sonnet-4-20250514', max_tokens:800,
        system:'生成一個模擬的 Uber Eats 訂單 JSON 陣列，含 2-3 個食物品項（type:"food"）和外送費、服務費（type:"fee"）。格式：[{"name":"品項名","amount":數字,"type":"food|fee"}]。只回傳 JSON。',
        messages:[{role:'user',content:'生成示範 Uber Eats 訂單'}]
      })
    });
    const data = await resp.json();
    const text = data.content[0].text.replace(/```json|```/g,'').trim();
    state.pendingItems = JSON.parse(text).map(i=>({...i,bearer:'split',xuanPct:50}));
  } catch(e) {
    state.pendingItems = [
      {name:'雞腿便當',amount:120,type:'food',bearer:'split',xuanPct:50},
      {name:'珍珠奶茶',amount:65,type:'food',bearer:'split',xuanPct:50},
      {name:'外送費',amount:49,type:'fee',bearer:'split',xuanPct:50},
      {name:'服務費',amount:15,type:'fee',bearer:'split',xuanPct:50},
    ];
  }
  renderPendingItems();
  btn.disabled = false;
  btn.innerHTML = '↓ 從 Uber Eats 抓取最新訂單';
}

function renderPendingItems() {
  const wrap = document.getElementById('order-items-wrap');
  if(!state.pendingItems.length) { wrap.innerHTML='<div class="empty">尚無訂單品項</div>'; return; }
  let html = `<div class="card"><div class="card-title">今日 UberEats 訂單 <span style="font-size:12px;color:var(--text3);font-weight:400;">暄先付</span></div>`;
  state.pendingItems.forEach((item, i) => {
    html += `<div class="split-item">
      <div class="split-item-top">
        <span class="split-item-name">${item.name}${item.type==='fee'?'<span class="badge-fee">費用</span>':''}</span>
        <span class="split-item-amt">NT$${item.amount}</span>
      </div>
      <div class="person-toggle">
        <button class="ptog ${item.bearer==='xuan'?'sel-xuan':''}" onclick="setPending(${i},'xuan')">暄</button>
        <button class="ptog ${item.bearer==='xiang'?'sel-xiang':''}" onclick="setPending(${i},'xiang')">祥</button>
        <button class="ptog ${item.bearer==='split'?'sel-split':''}" onclick="setPending(${i},'split')">各半</button>
      </div>
      ${item.bearer==='split'?`<div class="pct-row"><span class="pct-label" id="pct-l-${i}">暄 ${item.xuanPct}%</span><input type="range" min="0" max="100" step="1" value="${item.xuanPct}" oninput="setPendingPct(${i},+this.value)"><span class="pct-label" id="pct-r-${i}" style="text-align:right">祥 ${100-item.xuanPct}%</span></div>`:''}
    </div>`;
  });
  html += `</div>`;
  wrap.innerHTML = html;
  updateOrderSummary();
  document.getElementById('order-summary').style.display = 'block';
}

function setPending(i, bearer) { state.pendingItems[i].bearer = bearer; renderPendingItems(); }
function setPendingPct(i, val) {
  state.pendingItems[i].xuanPct = val;
  const l = document.getElementById('pct-l-'+i), r = document.getElementById('pct-r-'+i);
  if(l) l.textContent = `暄 ${val}%`;
  if(r) r.textContent = `祥 ${100-val}%`;
  updateOrderSummary();
}

function updateOrderSummary() {
  let xuan=0, xiang=0;
  state.pendingItems.forEach(item => { const a = computeAmounts(item.amount,item.bearer,item.xuanPct); xuan+=a.xuanAmt; xiang+=a.xiangAmt; });
  document.getElementById('xuan-total').textContent = `NT$${xuan}`;
  document.getElementById('xiang-total').textContent = `NT$${xiang}`;
}

function saveOrder() {
  if(!state.pendingItems.length) return;
  state.pendingItems.forEach(item => {
    const {xuanAmt, xiangAmt} = computeAmounts(item.amount, item.bearer, item.xuanPct);
    state.entries.push({ id:Date.now()+Math.random(), date:todayKey(), label:`${today()} UberEats`, platform:'UberEats', payer:'暄', name:item.name, amount:item.amount, xuanAmt, xiangAmt, bearer:item.bearer });
  });
  state.pendingItems = [];
  save();
  document.getElementById('order-items-wrap').innerHTML = '<div class="empty">已存入記帳表！</div>';
  document.getElementById('order-summary').style.display = 'none';
}

function clearOrder() {
  state.pendingItems = [];
  document.getElementById('order-items-wrap').innerHTML = '<div class="empty">尚無訂單品項</div>';
  document.getElementById('order-summary').style.display = 'none';
}

function renderLedger() {
  const wrap = document.getElementById('ledger-wrap');
  if(!state.entries.length) { wrap.innerHTML='<div class="empty">尚無記帳紀錄</div>'; return; }
  const grouped = {};
  state.entries.forEach(e => { if(!grouped[e.label]) grouped[e.label]=[]; grouped[e.label].push(e); });
  let html = '';
  Object.keys(grouped).sort().reverse().forEach(label => {
    const items = grouped[label];
    const total = items.reduce((s,e)=>s+e.amount,0);
    const xuanT = items.reduce((s,e)=>s+e.xuanAmt,0);
    const xiangT = items.reduce((s,e)=>s+e.xiangAmt,0);
    const plat = items[0].platform;
    const pillCls = plat==='UberEats'?'pill-ub':plat==='Foodpanda'?'pill-fp':'pill-manual';
    html += `<div class="card">
      <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
        <span class="pill ${pillCls}">${plat}</span>
        <span style="font-size:14px;font-weight:600;">${label}</span>
        <span style="font-size:12px;color:var(--text3);margin-left:auto;">先付：${items[0].payer}</span>
      </div>
      <div class="ledger-entry hdr"><span>品項</span><span style="text-align:right">金額</span><span style="text-align:right">暄</span><span style="text-align:right">祥</span><span></span></div>`;
    items.forEach(e => {
      html += `<div class="ledger-entry">
        <span style="font-size:12px">${e.name}</span>
        <span style="text-align:right;font-size:12px">$${e.amount}</span>
        <span style="text-align:right;font-size:12px;color:var(--green)">$${e.xuanAmt}</span>
        <span style="text-align:right;font-size:12px;color:var(--blue)">$${e.xiangAmt}</span>
        <button class="btn-danger" onclick="deleteEntry(${JSON.stringify(e.id)})">刪</button>
      </div>`;
    });
    html += `<div class="ledger-totals"><span>$${total}</span><span style="color:var(--green)">暄 $${xuanT}</span><span style="color:var(--blue)">祥 $${xiangT}</span></div></div>`;
  });
  wrap.innerHTML = html;
}

function deleteEntry(id) { state.entries = state.entries.filter(e=>e.id!==id); save(); renderLedger(); }

function calcMonthly() {
  const m = document.getElementById('month-picker').value;
  if(!m) return;
  const [yr,mo] = m.split('-');
  const filtered = state.entries.filter(e=>e.date.startsWith(`${yr}-${mo}`));
  const res = document.getElementById('monthly-result');
  if(!filtered.length) { res.innerHTML='<p style="font-size:13px;color:var(--text2)">該月份尚無資料。</p>'; return; }

  const xuanShouldPay = filtered.reduce((s,e)=>s+e.xuanAmt,0);
  const xiangShouldPay = filtered.reduce((s,e)=>s+e.xiangAmt,0);
  const xuanPaid = filtered.filter(e=>e.payer==='暄').reduce((s,e)=>s+e.amount,0);
  const grandTotal = filtered.reduce((s,e)=>s+e.amount,0);
  const xuanBalance = xuanPaid - xuanShouldPay;
  let settlement, transferAmt=0;
  if(Math.abs(xuanBalance)<1) settlement='兩人已結清，無需轉帳！';
  else if(xuanBalance>0) { transferAmt=Math.round(xuanBalance); settlement=`祥 需轉 NT$${transferAmt} 給 暄`; }
  else { transferAmt=Math.round(-xuanBalance); settlement=`暄 需轉 NT$${transferAmt} 給 祥`; }

  const monthLabel = `${yr} 年 ${parseInt(mo)} 月`;
  const breakdownRows = filtered.map(e=>`<tr><td style="padding:4px 6px;border-bottom:1px solid #eee;font-size:12px">${e.date}</td><td style="padding:4px 6px;border-bottom:1px solid #eee;font-size:12px">${e.platform}</td><td style="padding:4px 6px;border-bottom:1px solid #eee;font-size:12px">${e.name}</td><td style="padding:4px 6px;border-bottom:1px solid #eee;text-align:right;font-size:12px">NT$${e.amount}</td><td style="padding:4px 6px;border-bottom:1px solid #eee;text-align:right;font-size:12px;color:#0F6E56">NT$${e.xuanAmt}</td><td style="padding:4px 6px;border-bottom:1px solid #eee;text-align:right;font-size:12px;color:#185FA5">NT$${e.xiangAmt}</td></tr>`).join('');

  const htmlBody = `<div style="font-family:sans-serif;max-width:600px;margin:0 auto;padding:24px;"><h2 style="font-size:20px;margin-bottom:4px;">暄 &amp; 祥 ${monthLabel}記帳結算</h2><p style="color:#888;font-size:13px;margin-bottom:20px;">共 ${filtered.length} 筆消費</p><table style="width:100%;border-collapse:collapse;margin-bottom:16px;"><tr style="background:#f5f5f5"><td style="padding:8px;font-weight:600">本月總消費</td><td style="padding:8px;text-align:right;font-weight:600">NT$${grandTotal}</td></tr><tr><td style="padding:8px;color:#0F6E56">暄 應付</td><td style="padding:8px;text-align:right;color:#0F6E56;font-weight:600">NT$${xuanShouldPay}</td></tr><tr><td style="padding:8px;color:#185FA5">祥 應付</td><td style="padding:8px;text-align:right;color:#185FA5;font-weight:600">NT$${xiangShouldPay}</td></tr></table><div style="background:#f0faf5;padding:14px;border-radius:8px;font-weight:600;margin-bottom:20px;">${settlement}</div><h3 style="font-size:14px;margin-bottom:8px;">消費明細</h3><table style="width:100%;border-collapse:collapse;"><tr style="background:#f5f5f5"><th style="padding:4px 6px;text-align:left;font-size:12px">日期</th><th style="padding:4px 6px;text-align:left;font-size:12px">平台</th><th style="padding:4px 6px;text-align:left;font-size:12px">品項</th><th style="padding:4px 6px;text-align:right;font-size:12px">金額</th><th style="padding:4px 6px;text-align:right;font-size:12px;color:#0F6E56">暄</th><th style="padding:4px 6px;text-align:right;font-size:12px;color:#185FA5">祥</th></tr>${breakdownRows}</table><p style="font-size:12px;color:#aaa;margin-top:20px;">由 暄＆祥記帳本 自動產生</p></div>`;

  res.innerHTML = `
    <div class="metric-grid">
      <div class="metric"><div class="metric-label">本月總消費</div><div class="metric-value">NT$${grandTotal}</div></div>
      <div class="metric"><div class="metric-label">共幾筆</div><div class="metric-value">${filtered.length} 筆</div></div>
      <div class="metric"><div class="metric-label">暄 應付</div><div class="metric-value" style="color:var(--green)">NT$${xuanShouldPay}</div></div>
      <div class="metric"><div class="metric-label">祥 應付</div><div class="metric-value" style="color:var(--blue)">NT$${xiangShouldPay}</div></div>
    </div>
    <div class="banner-success" style="font-weight:600;margin-bottom:12px;">${settlement}</div>
    <button class="btn btn-gmail btn-full" id="send-btn" onclick="sendGmail('${monthLabel.replace(/'/g,"\\'")}',${xuanShouldPay},${xiangShouldPay},'${settlement.replace(/'/g,"\\'")}',\`${htmlBody.replace(/`/g,'\\`')}\`)">寄送月結算信到 yihsuan890101@gmail.com</button>
    <div id="gmail-status"></div>
  `;
}

async function sendGmail(monthLabel, xuanAmt, xiangAmt, settlement, htmlBody) {
  const btn = document.getElementById('send-btn');
  const status = document.getElementById('gmail-status');
  btn.disabled = true;
  btn.innerHTML = '<span class="dots"><span></span><span></span><span></span></span> 寄送中...';
  try {
    const resp = await fetch('https://api.anthropic.com/v1/messages', {
      method:'POST', headers:{'Content-Type':'application/json'},
      body: JSON.stringify({
        model:'claude-sonnet-4-20250514', max_tokens:1000,
        mcp_servers:[{type:'url',url:'https://gmailmcp.googleapis.com/mcp/v1',name:'gmail-mcp'}],
        system:`你是記帳助手。使用 Gmail MCP 建立 email draft：to: yihsuan890101@gmail.com, subject: 【${monthLabel}記帳結算】暄&祥, htmlBody 如下。建立完成後只回覆「草稿建立成功」。`,
        messages:[{role:'user',content:`請建立 Gmail draft，htmlBody: ${htmlBody}`}]
      })
    });
    const data = await resp.json();
    const txt = JSON.stringify(data);
    if(txt.includes('成功')||txt.includes('draft')||txt.includes('Draft')||txt.includes('created')) {
      status.innerHTML = '<div class="banner-success" style="margin-top:8px;">Gmail 草稿已建立！請到 Gmail 草稿夾確認後送出。</div>';
      btn.innerHTML = '草稿已建立 ✓'; btn.className='btn btn-full';
    } else { throw new Error(); }
  } catch(e) {
    status.innerHTML = '<div class="banner-warn" style="margin-top:8px;">建立草稿失敗，請確認 Gmail 連線後重試。</div>';
    btn.disabled = false; btn.innerHTML = '重試寄送月結算信';
  }
}

function enableReminder() {
  if(!('Notification' in window)) { alert('此瀏覽器不支援通知'); return; }
  Notification.requestPermission().then(perm => {
    if(perm==='granted') {
      document.getElementById('reminder-status').innerHTML = '<div class="banner-success" style="margin-top:8px;">已開啟！每天晚上 10 點提醒記帳。</div>';
      scheduleReminder();
    } else {
      document.getElementById('reminder-status').innerHTML = '<p style="font-size:13px;color:var(--red);margin-top:8px;">請在瀏覽器設定中允許通知。</p>';
    }
  });
}

function scheduleReminder() {
  const now = new Date(), target = new Date(now);
  target.setHours(22,0,0,0);
  if(now >= target) target.setDate(target.getDate()+1);
  setTimeout(()=>{
    new Notification('📒 記帳提醒', {body:'今天的外送費還沒記帳喔！'});
    setInterval(()=>new Notification('📒 記帳提醒', {body:'今天的外送費還沒記帳喔！'}), 86400000);
  }, target - now);
}

function clearAll() {
  if(confirm('確定要清除所有記帳紀錄嗎？此動作無法復原。')) {
    state = {entries:[], pendingItems:[]};
    save();
    renderLedger();
    alert('已清除。');
  }
}

save();
</script>
</body>
</html>
