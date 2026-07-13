<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Painel de Expedição</title>
<style>
  :root{
    --ink: #1c2430;
    --paper: #eef1f0;
    --card: #ffffff;
    --steel: #2f4858;
    --steel-dark: #1e313d;
    --amber: #c97a2c;
    --amber-bg: #fbe9d4;
    --green: #2f7a4f;
    --green-bg: #dcefe1;
    --line: #d7dcd9;
    --mono: 'JetBrains Mono', 'Courier New', monospace;
    --sans: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  body{
    background: var(--paper);
    background-image:
      linear-gradient(var(--line) 1px, transparent 1px),
      linear-gradient(90deg, var(--line) 1px, transparent 1px);
    background-size: 28px 28px;
    color: var(--ink);
    font-family: var(--sans);
    min-height: 100vh;
    padding: 32px 20px 80px;
  }
  .wrap{ max-width: 1100px; margin: 0 auto; }

  header{
    display:flex;
    justify-content:space-between;
    align-items:flex-end;
    flex-wrap:wrap;
    gap: 16px;
    border-bottom: 3px solid var(--ink);
    padding-bottom: 18px;
    margin-bottom: 28px;
  }
  .brand-eyebrow{
    font-family: var(--mono);
    font-size: 12px;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--steel);
    margin-bottom: 6px;
  }
  h1{
    font-size: 32px;
    font-weight: 800;
    letter-spacing: -0.01em;
  }
  .stamp{
    font-family: var(--mono);
    font-size: 12px;
    color: #6b7680;
    text-align:right;
  }
  #synctag{
    display:inline-block;
    margin-top:4px;
    font-size:11px;
    padding: 2px 8px;
    border: 1px solid var(--line);
    border-radius: 3px;
    color: var(--steel);
    background: var(--card);
  }

  .new-order{
    background: var(--card);
    border: 1px solid var(--line);
    border-radius: 10px;
    padding: 18px;
    margin-bottom: 32px;
    box-shadow: 0 1px 0 rgba(0,0,0,0.03);
  }
  .new-order-title{
    font-family: var(--mono);
    font-size: 11px;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--steel);
    margin-bottom: 12px;
  }
  .new-order-row{
    display:flex;
    gap: 12px;
    flex-wrap: wrap;
  }
  .new-order-row input{
    font-family: var(--sans);
    font-size: 14px;
    padding: 10px 12px;
    border: 1px solid var(--line);
    border-radius: 6px;
    background: var(--paper);
    color: var(--ink);
  }
  .new-order-row input[type="text"]{ flex: 1 1 260px; }
  .new-order-row input[type="date"]{ flex: 0 0 170px; }
  .new-order-row input:focus{ outline: 2px solid var(--steel); outline-offset: 1px; }
  .add-btn{
    font-family: var(--sans);
    font-weight: 700;
    font-size: 14px;
    padding: 10px 20px;
    border: none;
    border-radius: 6px;
    background: var(--ink);
    color: #fff;
    cursor: pointer;
    transition: transform 0.05s ease, background 0.15s ease;
  }
  .add-btn:hover{ background: var(--steel-dark); }
  .add-btn:active{ transform: translateY(1px); }

  .board-head{
    display:flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 12px;
  }
  .board-head h2{
    font-size: 13px;
    font-family: var(--mono);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--steel);
  }
  .count-pill{
    font-family: var(--mono);
    font-size: 12px;
    color: #6b7680;
  }

  .order-card{
    background: var(--card);
    border: 1px solid var(--line);
    border-left: 5px solid var(--line);
    border-radius: 8px;
    padding: 16px 18px;
    margin-bottom: 12px;
    transition: border-left-color 0.2s ease;
  }
  .order-card.complete{ border-left-color: var(--green); }
  .order-card.pending{ border-left-color: var(--amber); }

  .order-top{
    display:flex;
    justify-content: space-between;
    align-items:flex-start;
    gap: 12px;
    margin-bottom: 14px;
  }
  .order-name-block{ flex: 1; min-width: 180px; }
  .order-name{
    font-size: 17px;
    font-weight: 700;
    line-height: 1.3;
  }
  .order-date{
    font-family: var(--mono);
    font-size: 12px;
    color: #6b7680;
    margin-top: 3px;
  }
  .order-actions{ display:flex; align-items:center; gap: 10px; }
  .progress-badge{
    font-family: var(--mono);
    font-size: 11px;
    padding: 3px 9px;
    border-radius: 20px;
    white-space: nowrap;
  }
  .progress-badge.complete{ background: var(--green-bg); color: var(--green); }
  .progress-badge.pending{ background: var(--amber-bg); color: var(--amber); }

  .del-btn{
    background: none;
    border: 1px solid var(--line);
    color: #9aa3a0;
    font-size: 12px;
    width: 26px;
    height: 26px;
    border-radius: 50%;
    cursor: pointer;
    line-height: 1;
    transition: all 0.15s ease;
  }
  .del-btn:hover{ background:#fbdada; color:#b23a3a; border-color:#f0b6b6; }

  .steps-grid{
    display:grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
  }
  @media (max-width: 640px){
    .steps-grid{ grid-template-columns: repeat(2, 1fr); }
  }

  .step-toggle{
    display:flex;
    flex-direction: column;
    gap: 6px;
    border: 1px solid var(--line);
    border-radius: 7px;
    padding: 10px 12px;
    cursor: pointer;
    background: var(--paper);
    transition: background 0.15s ease, border-color 0.15s ease;
    user-select: none;
  }
  .step-label{
    font-family: var(--mono);
    font-size: 10px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: #6b7680;
  }
  .step-value{
    font-size: 13px;
    font-weight: 700;
    display:flex;
    align-items:center;
    gap: 6px;
  }
  .dot{
    width: 8px; height: 8px; border-radius: 50%;
    background: #b9c0bd;
    flex-shrink:0;
  }
  .step-toggle.done{ background: var(--green-bg); border-color: #bfe0cb; }
  .step-toggle.done .dot{ background: var(--green); }
  .step-toggle.done .step-value{ color: var(--green); }

  .step-toggle.waiting{ background: var(--amber-bg); border-color: #ecd3ab; }
  .step-toggle.waiting .dot{ background: var(--amber); }
  .step-toggle.waiting .step-value{ color: var(--amber); }

  .empty-state{
    text-align:center;
    padding: 60px 20px;
    color: #8a938f;
    font-family: var(--mono);
    font-size: 13px;
    border: 1px dashed var(--line);
    border-radius: 10px;
  }

  footer{
    text-align:center;
    margin-top: 40px;
    font-family: var(--mono);
    font-size: 11px;
    color: #9aa3a0;
  }
</style>
</head>
<body>
<div class="wrap">

  <header>
    <div>
      <div class="brand-eyebrow">Controle de expedição</div>
      <h1>Painel de Pedidos</h1>
    </div>
    <div class="stamp">
      Atualizado ao vivo<br>
      <span id="synctag">carregando…</span>
    </div>
  </header>

  <div class="new-order">
    <div class="new-order-title">Novo pedido</div>
    <div class="new-order-row">
      <input type="text" id="input-name" placeholder="Nome do pedido">
      <input type="date" id="input-date">
      <button class="add-btn" id="btn-add">Adicionar</button>
    </div>
  </div>

  <div class="board-head">
    <h2>Pedidos em andamento</h2>
    <span class="count-pill" id="count-pill"></span>
  </div>

  <div id="orders-list"></div>

</div>

<footer>Dados compartilhados — visíveis para todos que acessam este painel</footer>

<script>
const STORAGE_KEY_PREFIX = 'pedido:';
let orders = [];

function todayStr(){
  const d = new Date();
  return d.toISOString().slice(0,10);
}
document.getElementById('input-date').value = todayStr();

function formatDate(dateStr){
  if(!dateStr) return '';
  const [y,m,d] = dateStr.split('-');
  return `${d}/${m}/${y}`;
}

function uid(){
  return 'o' + Date.now().toString(36) + Math.random().toString(36).slice(2,7);
}

async function loadOrders(){
  const synctag = document.getElementById('synctag');
  try{
    const list = await window.storage.list(STORAGE_KEY_PREFIX, true);
    const keys = (list && list.keys) ? list.keys : [];
    const loaded = [];
    for(const key of keys){
      try{
        const res = await window.storage.get(key, true);
        if(res && res.value){
          loaded.push(JSON.parse(res.value));
        }
      }catch(e){ /* skip missing */ }
    }
    loaded.sort((a,b) => (b.createdAt||0) - (a.createdAt||0));
    orders = loaded;
    synctag.textContent = 'sincronizado';
  }catch(e){
    synctag.textContent = 'erro ao sincronizar';
    console.error('Erro ao carregar pedidos', e);
  }
  render();
}

async function saveOrder(order){
  try{
    await window.storage.set(STORAGE_KEY_PREFIX + order.id, JSON.stringify(order), true);
  }catch(e){
    console.error('Erro ao salvar pedido', e);
    alert('Não foi possível salvar o pedido. Tente novamente.');
  }
}

async function deleteOrder(id){
  try{
    await window.storage.delete(STORAGE_KEY_PREFIX + id, true);
    orders = orders.filter(o => o.id !== id);
    render();
  }catch(e){
    console.error('Erro ao excluir pedido', e);
    alert('Não foi possível excluir o pedido. Tente novamente.');
  }
}

async function addOrder(){
  const nameInput = document.getElementById('input-name');
  const dateInput = document.getElementById('input-date');
  const name = nameInput.value.trim();
  if(!name){
    nameInput.focus();
    return;
  }
  const order = {
    id: uid(),
    name: name,
    date: dateInput.value || todayStr(),
    nfe: false,
    boleto: false,
    estoque: 'aguardando',
    entrega: false,
    createdAt: Date.now()
  };
  orders.unshift(order);
  render();
  nameInput.value = '';
  dateInput.value = todayStr();
  nameInput.focus();
  await saveOrder(order);
}

function isComplete(o){
  return o.nfe && o.boleto && o.estoque === 'separado' && o.entrega;
}

function render(){
  const list = document.getElementById('orders-list');
  const countPill = document.getElementById('count-pill');
  const doneCount = orders.filter(isComplete).length;
  countPill.textContent = orders.length ? `${doneCount} de ${orders.length} concluídos` : '';

  if(orders.length === 0){
    list.innerHTML = '<div class="empty-state">Nenhum pedido ainda. Adicione o primeiro acima.</div>';
    return;
  }

  list.innerHTML = orders.map(o => {
    const complete = isComplete(o);
    return `
    <div class="order-card ${complete ? 'complete' : 'pending'}" data-id="${o.id}">
      <div class="order-top">
        <div class="order-name-block">
          <div class="order-name">${escapeHtml(o.name)}</div>
          <div class="order-date">Pedido em ${formatDate(o.date)}</div>
        </div>
        <div class="order-actions">
          <span class="progress-badge ${complete ? 'complete' : 'pending'}">${complete ? 'Concluído' : 'Em andamento'}</span>
          <button class="del-btn" data-action="delete" title="Excluir pedido">✕</button>
        </div>
      </div>
      <div class="steps-grid">
        <div class="step-toggle ${o.nfe ? 'done' : 'waiting'}" data-action="toggle-nfe">
          <span class="step-label">NF-e</span>
          <span class="step-value"><span class="dot"></span>${o.nfe ? 'Feito' : 'Não feito'}</span>
        </div>
        <div class="step-toggle ${o.boleto ? 'done' : 'waiting'}" data-action="toggle-boleto">
          <span class="step-label">Boleto</span>
          <span class="step-value"><span class="dot"></span>${o.boleto ? 'Feito' : 'Não feito'}</span>
        </div>
        <div class="step-toggle ${o.estoque === 'separado' ? 'done' : 'waiting'}" data-action="toggle-estoque">
          <span class="step-label">Estoque</span>
          <span class="step-value"><span class="dot"></span>${o.estoque === 'separado' ? 'Separado' : 'Aguardando'}</span>
        </div>
        <div class="step-toggle ${o.entrega ? 'done' : 'waiting'}" data-action="toggle-entrega">
          <span class="step-label">Entrega</span>
          <span class="step-value"><span class="dot"></span>${o.entrega ? 'Feito' : 'Não feito'}</span>
        </div>
      </div>
    </div>
    `;
  }).join('');
}

function escapeHtml(str){
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

document.getElementById('btn-add').addEventListener('click', addOrder);
document.getElementById('input-name').addEventListener('keydown', (e) => {
  if(e.key === 'Enter') addOrder();
});

document.getElementById('orders-list').addEventListener('click', async (e) => {
  const card = e.target.closest('.order-card');
  if(!card) return;
  const id = card.dataset.id;
  const order = orders.find(o => o.id === id);
  if(!order) return;

  const actionEl = e.target.closest('[data-action]');
  if(!actionEl) return;
  const action = actionEl.dataset.action;

  if(action === 'delete'){
    if(confirm(`Excluir o pedido "${order.name}"?`)){
      await deleteOrder(id);
    }
    return;
  }
  if(action === 'toggle-nfe') order.nfe = !order.nfe;
  if(action === 'toggle-boleto') order.boleto = !order.boleto;
  if(action === 'toggle-estoque') order.estoque = order.estoque === 'separado' ? 'aguardando' : 'separado';
  if(action === 'toggle-entrega') order.entrega = !order.entrega;

  render();
  await saveOrder(order);
});

loadOrders();
</script>
</body>
</html>
