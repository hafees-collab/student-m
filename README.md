<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Student Money</title>
<style>
:root{--bg:#f4f6f5;--card:#fff;--tx:#151a18;--mu:#6c7774;--bd:#e3e8e6;--ac:#2b6b5a;--in:#16803c;--out:#c62828;--wa:#b45309;--bl:#1d4ed8}
[data-t=dark]{--bg:#0e1211;--card:#161c1a;--tx:#e9eeec;--mu:#8a9693;--bd:#262f2c;--ac:#4fb59a;--in:#4ade80;--out:#f87171;--wa:#fbbf24;--bl:#60a5fa}
*{box-sizing:border-box;margin:0;-webkit-tap-highlight-color:transparent}
body{font:15px/1.4 system-ui,-apple-system,"Segoe UI",sans-serif;background:var(--bg);color:var(--tx);padding-top:env(safe-area-inset-top)}
#app{max-width:560px;margin:auto;padding:16px 16px 150px}
small,.mu{color:var(--mu);font-size:13px}
h1{font-size:38px;letter-spacing:-1px;margin:2px 0 8px}h2{font-size:13px;color:var(--mu);text-transform:uppercase;letter-spacing:.6px;margin:20px 0 8px}
.card,.hero{background:var(--card);border:1px solid var(--bd);border-radius:14px;padding:14px}
.hero{background:var(--ac);color:#fff;border:0;padding:18px}.hero small{color:#ffffffcc}.hero .r{display:flex;gap:18px;font-weight:600}.hero .sv{margin-top:10px;font-size:13px;opacity:.85}
[data-t=dark] .hero{color:#07120f}[data-t=dark] .hero small{color:#07120fb0}
.seg{display:flex;background:var(--card);border:1px solid var(--bd);border-radius:10px;padding:3px;margin:16px 0 10px}
.seg button{flex:1;border:0;background:none;color:var(--mu);padding:8px;border-radius:8px;font:600 14px inherit}.seg .on{background:var(--ac);color:#fff}
.g{display:grid;grid-template-columns:1fr 1fr;gap:10px}.g b{font-size:20px;display:block}
.in{color:var(--in)}.out{color:var(--out)}.neu{color:var(--tx)}
.qa{display:grid;grid-template-columns:repeat(5,1fr);gap:8px}
.qa button{background:var(--card);border:1px solid var(--bd);border-radius:12px;padding:10px 2px;color:var(--tx);font:600 11px inherit;display:flex;flex-direction:column;gap:4px;align-items:center;min-height:62px}.qa i{font-style:normal;font-size:20px;color:var(--ac)}
.p{display:flex;gap:10px;padding:8px 0}.p+.p{border-top:1px solid var(--bd)}
.tx{display:flex;justify-content:space-between;gap:10px;padding:12px 0;border-bottom:1px solid var(--bd);cursor:pointer}.tx b{white-space:nowrap}
.bar{height:8px;border-radius:4px;background:var(--bd);margin:4px 0 10px;overflow:hidden}.bar i{display:block;height:100%;background:var(--ac)}
.row{display:flex;justify-content:space-between;align-items:center;gap:8px}
input,select{width:100%;padding:12px;border:1px solid var(--bd);border-radius:10px;background:var(--bg);color:var(--tx);font:16px inherit;margin:4px 0 12px}
.f{display:grid;grid-template-columns:1fr 1fr;gap:8px}.f input,.f select{margin:0 0 8px}
.b{border:0;border-radius:10px;padding:12px 16px;font:600 15px inherit;background:var(--ac);color:#fff;width:100%}.b.s{background:var(--bd);color:var(--tx)}.b.d{background:none;color:var(--out)}.sm{width:auto;padding:7px 12px;font-size:13px}
nav{position:fixed;bottom:0;left:0;right:0;background:var(--card);border-top:1px solid var(--bd);display:flex;padding-bottom:env(safe-area-inset-bottom);z-index:5}
nav button{flex:1;border:0;background:none;color:var(--mu);padding:9px 0;font:600 11px inherit;display:flex;flex-direction:column;gap:2px;align-items:center}nav i{font-style:normal;font-size:19px}nav .on{color:var(--ac)}
#fab{position:fixed;right:max(16px,calc(50% - 264px));bottom:calc(76px + env(safe-area-inset-bottom));background:var(--ac);color:#fff;border:0;border-radius:28px;padding:14px 20px;font:700 15px inherit;box-shadow:0 4px 14px #0004;z-index:5}
#m{position:fixed;inset:0;background:#0007;display:none;align-items:flex-end;justify-content:center;z-index:9}
#m>div{background:var(--card);width:100%;max-width:560px;border-radius:18px 18px 0 0;padding:18px 18px calc(18px + env(safe-area-inset-bottom));max-height:92vh;overflow:auto}
#m h3{margin-bottom:12px}label{font-size:13px;color:var(--mu)}
#lock{position:fixed;inset:0;background:var(--bg);z-index:20;display:none;flex-direction:column;align-items:center;justify-content:center;gap:10px}#lock input{width:160px;text-align:center;font-size:24px;letter-spacing:8px}
#t{position:fixed;top:14px;left:50%;transform:translateX(-50%);background:#222;color:#fff;padding:9px 16px;border-radius:20px;font-size:14px;display:none;z-index:30}
</style></head><body>
<div id="app"></div>
<button id="fab" onclick="openForm('EXPENSE')">− Expense</button>
<nav id="nav"></nav><div id="m" onclick="if(event.target==this)closeM()"><div id="mb"></div></div>
<div id="lock"><h3>🔒 Enter PIN</h3><input id="pin" type="password" inputmode="numeric" maxlength="4" oninput="unlock()"></div><div id="t"></div>
<script>
const $=s=>document.querySelector(s),ACC=['Cash','UPI / GP','Savings'],CATS=['Food','Transport','Other','Unexpected'],SRC=['Pocket Money','Side Hustle','Gift','Refund','Reimbursement','Other'];
const T={INCOME:['Money In'],EXPENSE:['Expense'],TRANSFER:['Transfer'],CREDIT_GIVEN:['I lent money'],CREDIT_RECEIVED:['Repaid to me'],DEBT_TAKEN:['I borrowed'],DEBT_REPAID:['I repaid']};
const ls=(k,d)=>{try{return JSON.parse(localStorage.getItem(k))??d}catch{return d}};
const S={tx:ls('tx',[]),open:ls('open',{}),tab:'home',per:'week'},F={q:'',t:'',a:'',p:'all'};
const esc=s=>String(s??'').replace(/[&<>"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
const inr=n=>(n<0?'−':'')+'₹'+Math.abs(Math.round(n*100)/100).toLocaleString('en-IN');
const today=()=>new Date().toLocaleDateString('en-CA'),fmt=d=>new Date(d+'T00:00:00').toLocaleDateString('en-IN',{day:'numeric',month:'short'});
const add=(d,n)=>{const x=new Date(d+'T00:00:00');x.setDate(x.getDate()+n);return x.toLocaleDateString('en-CA')};
const save=()=>{localStorage.setItem('tx',JSON.stringify(S.tx));localStorage.setItem('open',JSON.stringify(S.open))};
const toast=m=>{const t=$('#t');t.textContent=m;t.style.display='block';setTimeout(()=>t.style.display='none',2200)};

/* ---------- date ranges (week starts Monday) ---------- */
function range(per,off=0){const d=new Date();d.setHours(0,0,0,0);let a,b;
 if(per=='week'){a=new Date(d);a.setDate(d.getDate()-(d.getDay()+6)%7+off*7);b=new Date(a);b.setDate(a.getDate()+6)}
 else{a=new Date(d.getFullYear(),d.getMonth()+off,1);b=new Date(d.getFullYear(),d.getMonth()+off+1,0)}
 const f=x=>x.toLocaleDateString('en-CA');return[f(a),f(b)]}
const inR=(t,r)=>t.date>=r[0]&&t.date<=r[1];
const elapsed=r=>Math.max(1,Math.round((new Date(today())-new Date(r[0]))/864e5)+1);

/* ---------- ALL numbers are calculated from the ledger ---------- */
function calc(){const b={Cash:0,'UPI / GP':0,Savings:0,...S.open},rec={},pay={},a=(o,k,v)=>o[k]=(o[k]||0)+v;
 for(const t of S.tx){const n=t.amount;switch(t.type){
  case'INCOME':a(b,t.account,n);break;
  case'EXPENSE':a(b,t.account,-n);break;
  case'TRANSFER':a(b,t.account,-n);a(b,t.to,n);break;
  case'CREDIT_GIVEN':a(b,t.account,-n);a(rec,t.person,n);break;
  case'CREDIT_RECEIVED':a(b,t.account,n);a(rec,t.person,-n);break;
  case'DEBT_TAKEN':a(b,t.account,n);a(pay,t.person,n);break;
  case'DEBT_REPAID':a(b,t.account,-n);a(pay,t.person,-n)}}
 return{b,rec,pay}}
function sum(r){let i=0,e=0,s=0;for(const t of S.tx)if(inR(t,r)){
 if(t.type=='INCOME')i+=t.amount;else if(t.type=='EXPENSE')e+=t.amount;
 else if(t.type=='TRANSFER'){if(t.to=='Savings'&&t.account!='Savings')s+=t.amount;if(t.account=='Savings'&&t.to!='Savings')s-=t.amount}}
 return{i,e,s,n:i-e-s}}
function grp(r,type,key){const o={};S.tx.forEach(t=>{if(t.type==type&&inR(t,r))o[t[key]||'Other']=(o[t[key]||'Other']||0)+t.amount});return o}
function same(){const r=range(S.per),p=range(S.per,-1);return{r,pr:[p[0],add(p[0],elapsed(r)-1)]}} // fair comparison: same days so far

/* ---------- views ---------- */
const per=()=>`<div class=seg>${['week','month'].map(p=>`<button class="${S.per==p?'on':''}" onclick="S.per='${p}';render()">This ${p}</button>`).join('')}</div>`;
const bar=(l,v,max,extra='')=>`<div class=row><span>${esc(l)}</span><span>${inr(v)} <small>${extra}</small></span></div><div class=bar><i style="width:${max?v/max*100:0}%"></i></div>`;
function pulse(){const{r,pr}=same(),m=sum(r),p=sum(pr),nm=S.per,o=[],cm=grp(r,'EXPENSE','category'),cp=grp(pr,'EXPENSE','category');
 if(p.e>0&&m.e<p.e)o.push(['🟢','Good '+nm,`You spent ${inr(p.e-m.e)} less than the same days last ${nm}.`]);
 if(p.e>0&&m.e>p.e)o.push(['🟠','Watch your spending',`You spent ${inr(m.e-p.e)} more than the same days last ${nm}.`]);
 let top=null;for(const k in cm){const d=cm[k]-(cp[k]||0);if(cp[k]>0&&d>0&&(!top||d>top[1]))top=[k,d]}
 if(top)o.push(['🟠',top[0]+' is higher',`${inr(top[1])} more than the same days last ${nm}.`]);
 if(m.s>0)o.push(['🔵','Strong saving',`You moved ${inr(m.s)} into savings this ${nm}.`]);
 if(!o.length)o.push(['💡','Not enough data yet','Insights appear once you have a couple of weeks of entries.']);
 return o.map(x=>`<div class=p><span>${x[0]}</span><div><b>${x[1]}</b><br><small>${x[2]}</small></div></div>`).join('')}
const V={
home(){const c=calc(),m=sum(range(S.per)),tr=c.b.Cash+c.b['UPI / GP'];
 return`<div class=hero><small>Available to Spend</small><h1>${inr(tr)}</h1><div class=r><span>Cash ${inr(c.b.Cash)}</span><span>UPI / GP ${inr(c.b['UPI / GP'])}</span></div><div class=sv>Savings ${inr(c.b.Savings)} · kept separate</div></div>
 ${per()}<div class=g><div class=card><small>Money In</small><b class=in>${inr(m.i)}</b></div><div class=card><small>Spent</small><b class=out>${inr(m.e)}</b></div><div class=card><small>Saved</small><b>${inr(m.s)}</b></div><div class=card><small>Net Change</small><b class="${m.n<0?'out':'in'}">${m.n<0?'':'+'}${inr(m.n)}</b></div></div>
 <h2>Quick add</h2><div class=qa>${[['INCOME','+','Money In'],['EXPENSE','−','Expense'],['TRANSFER','⇄','Transfer'],['CREDIT_GIVEN','↗','Credit'],['DEBT_TAKEN','↘','Debt']].map(q=>`<button onclick="openForm('${q[0]}')"><i>${q[1]}</i>${q[2]}</button>`).join('')}</div>
 <h2>Money Pulse</h2><div class=card>${pulse()}</div>`},
tx(){return`<input placeholder="Search note, person, amount…" value="${esc(F.q)}" oninput="F.q=this.value;showList()">
 <div class=f><select onchange="F.t=this.value;showList()"><option value="">All types</option>${Object.keys(T).map(k=>`<option value=${k} ${F.t==k?'selected':''}>${T[k][0]}</option>`).join('')}</select>
 <select onchange="F.a=this.value;showList()"><option value="">All accounts</option>${ACC.map(a=>`<option ${F.a==a?'selected':''}>${a}</option>`).join('')}</select></div>
 <select onchange="F.p=this.value;showList()">${[['all','All time'],['week','This week'],['month','This month']].map(p=>`<option value=${p[0]} ${F.p==p[0]?'selected':''}>${p[1]}</option>`).join('')}</select><div id=lst></div>`},
money(){const c=calc(),pl=(o,t,btn)=>Object.entries(o).filter(e=>Math.abs(e[1])>0.004).map(([n,v])=>`<div class=row style="padding:8px 0"><span>${esc(n)} <b>${inr(v)}</b></span><button class="b s sm" onclick="openForm('${t}',{person:'${esc(n).replace(/'/g,'')}',amount:${v}})">${btn}</button></div>`).join('')||'<small>Nobody. All clear.</small>';
 const tr=Object.values(c.rec).reduce((a,b)=>a+b,0),td=Object.values(c.pay).reduce((a,b)=>a+b,0);
 return`<h2>Accounts</h2>${ACC.map(a=>`<div class="card row" style="margin-bottom:8px"><span>${a=='Cash'?'💵':a=='Savings'?'🏦':'📱'} ${a}</span><b>${inr(c.b[a])}</b></div>`).join('')}
 <div class=f><button class=b onclick="openForm('TRANSFER',{account:'Cash',to:'Savings'})">→ To Savings</button><button class="b s" onclick="openForm('TRANSFER',{account:'Savings',to:'Cash'})">← Withdraw</button></div>
 <small>Transfers only move your own money. They are not income or expenses.</small>
 <h2>Owed to me · ${inr(tr)}</h2><div class=card>${pl(c.rec,'CREDIT_RECEIVED','Got repaid')}<button class="b s sm" style="margin-top:6px" onclick="openForm('CREDIT_GIVEN')">+ Lend</button></div>
 <h2>I owe · ${inr(td)}</h2><div class=card>${pl(c.pay,'DEBT_REPAID','Repay')}<button class="b s sm" style="margin-top:6px" onclick="openForm('DEBT_TAKEN')">+ Borrow</button></div>`},
stats(){const{r,pr}=same(),m=sum(r),p=sum(pr),cm=grp(r,'EXPENSE','category'),src=grp(r,'INCOME','category'),el=elapsed(r),mx=Math.max(...Object.values(cm),1),
 wd=[0,0,0,0,0,0,0];S.tx.forEach(t=>{if(t.type=='EXPENSE'&&inR(t,r))wd[(new Date(t.date+'T00:00:00').getDay()+6)%7]+=t.amount});
 const d=p.e-m.e,days=['Mon','Tue','Wed','Thu','Fri','Sat','Sun'];
 return`${per()}<div class=g><div class=card><small>Money In</small><b class=in>${inr(m.i)}</b></div><div class=card><small>Spent</small><b class=out>${inr(m.e)}</b></div><div class=card><small>Saved</small><b>${inr(m.s)}</b></div><div class=card><small>Daily average</small><b>${inr(m.e/el)}</b></div></div>
 <h2>vs last ${S.per} (same days)</h2><div class=card>This ${S.per}: <b>${inr(m.e)}</b><br>Last ${S.per}: <b>${inr(p.e)}</b><br>${p.e?`<span class="${d>=0?'in':'out'}">${inr(Math.abs(d))} ${d>=0?'less':'more'} than last ${S.per}</span>`:'<small>No data from last '+S.per+' yet.</small>'}</div>
 <h2>Spending by category</h2><div class=card>${CATS.concat(Object.keys(cm).filter(k=>!CATS.includes(k))).map(k=>bar(k,cm[k]||0,mx,m.e?Math.round((cm[k]||0)/m.e*100)+'%':'0%')).join('')}</div>
 <h2>Spending by weekday</h2><div class=card>${days.map((n,i)=>bar(n,wd[i],Math.max(...wd,1))).join('')}</div>
 <h2>Money in by source</h2><div class=card>${Object.keys(src).map(k=>bar(k,src[k],m.i,Math.round(src[k]/m.i*100)+'%')).join('')||'<small>No income this '+S.per+'.</small>'}</div>`},
more(){const th=localStorage.th||'system',api=localStorage.api||'',pin=localStorage.pin;
 return`<h2>Theme</h2><div class=seg style="margin:0">${['light','dark','system'].map(t=>`<button class="${th==t?'on':''}" onclick="theme('${t}');render()">${{light:'☀️ Light',dark:'🌙 Dark',system:'Auto'}[t]}</button>`).join('')}</div>
 <h2>Google Sheets</h2><div class=card><small>Paste your Apps Script Web App URL (ends with /exec). Leave empty to use this device only.</small><input id=api value="${esc(api)}" placeholder="https://script.google.com/macros/s/…/exec"><div class=f><button class=b onclick="localStorage.api=$('#api').value.trim();load(true)">Save & Sync</button><button class="b s" onclick="load(true)">Sync now</button></div></div>
 ${api?'':`<h2>Opening balances (this device)</h2><div class=card>${ACC.map(a=>`<label>${a}</label><input type=number inputmode=decimal value="${S.open[a]||0}" onchange="S.open['${a}']=+this.value;save()">`).join('')}</div>`}
 <h2>PIN lock</h2><div class=card><small>A convenience lock to stop casual snooping on this device. It is not encryption and does not protect your Sheet. Real access control is your Apps Script deployment setting.</small><div class=f style="margin-top:8px"><input id=np type=password inputmode=numeric maxlength=4 placeholder="New 4-digit PIN"><button class=b onclick="setPin()">${pin?'Change':'Set'} PIN</button></div>${pin?'<button class="b d" onclick="localStorage.removeItem(\'pin\');render()">Remove PIN</button>':''}</div>`}};

function showList(){const q=F.q.toLowerCase(),r=F.p=='all'?null:range(F.p),a=S.tx.filter(t=>(!r||inR(t,r))&&(!F.t||t.type==F.t)&&(!F.a||t.account==F.a||t.to==F.a)&&(!q||(t.note+lab(t)+t.account+t.amount).toLowerCase().includes(q))).sort((x,y)=>y.date.localeCompare(x.date)||y.id.localeCompare(x.id));
 $('#lst').innerHTML=a.map(t=>{const s={INCOME:1,CREDIT_RECEIVED:1,DEBT_TAKEN:1,EXPENSE:-1,CREDIT_GIVEN:-1,DEBT_REPAID:-1}[t.type]||0;
  return`<div class=tx onclick="edit('${t.id}')"><div><div>${esc(t.note||lab(t))}</div><small>${fmt(t.date)} · ${t.note?esc(lab(t))+' · ':''}${t.type=='TRANSFER'?'Transfer':esc(t.account)}</small></div><b class="${s>0?'in':s<0?'out':'neu'}">${s>0?'+':s<0?'−':'⇄ '}${inr(t.amount).replace('−','')}</b></div>`}).join('')||'<p class=mu style="padding:16px 0">No transactions found.</p>'}
const lab=t=>({INCOME:t.category||'Income',EXPENSE:t.category,TRANSFER:t.account+' → '+t.to,CREDIT_GIVEN:'Lent to '+t.person,CREDIT_RECEIVED:t.person+' repaid me',DEBT_TAKEN:'Borrowed from '+t.person,DEBT_REPAID:'Repaid '+t.person}[t.type]||'');

/* ---------- add / edit form ---------- */
const sel=(id,arr,v)=>`<select id=${id}>${arr.map(x=>`<option ${x==v?'selected':''}>${esc(x)}</option>`).join('')}</select>`;
const L=(l,h)=>`<label>${l}</label>${h}`;
function openForm(type,p={}){const g=type.startsWith('CREDIT')?['CREDIT_GIVEN','CREDIT_RECEIVED']:type.startsWith('DEBT')?['DEBT_TAKEN','DEBT_REPAID']:0,
 ppl=[...new Set(S.tx.map(t=>t.person).filter(Boolean))];
 let h=`<h3>${p.id?'Edit':'Add'} · ${T[type][0]}</h3>`+L('Amount (₹)',`<input id=f_a type=number inputmode=decimal step=any value="${p.amount||''}" placeholder="0" autofocus>`);
 if(g)h+=L('Type',`<select id=f_ty>${g.map(x=>`<option value=${x} ${x==type?'selected':''}>${T[x][0]}</option>`).join('')}</select>`);
 if(type=='INCOME')h+=L('Source',sel('f_c',SRC,p.category));
 if(type=='EXPENSE')h+=L('Category',sel('f_c',CATS,p.category));
 if(g)h+=L('Person',`<input id=f_p list=pl value="${esc(p.person||'')}" placeholder="Name"><datalist id=pl>${ppl.map(n=>`<option>${esc(n)}</option>`).join('')}</datalist>`);
 h+=L(type=='TRANSFER'?'From':'Account',sel('f_ac',ACC,p.account||'Cash'));
 if(type=='TRANSFER')h+=L('To',sel('f_to',ACC,p.to||'Savings'));
 h+=L('Date',`<input id=f_d type=date value="${p.date||today()}">`)+L('Note',`<input id=f_n value="${esc(p.note||'')}" placeholder="Optional">`);
 h+=`<button class=b onclick="saveTx('${type}','${p.id||''}')">Save</button>${p.id?`<button class="b d" onclick="delTx('${p.id}')">Delete</button>`:''}<button class="b s" style="margin-top:8px" onclick="closeM()">Cancel</button>`;
 $('#mb').innerHTML=h;$('#m').style.display='flex';setTimeout(()=>$('#f_a').focus(),50)}
const closeM=()=>$('#m').style.display='none';
const edit=id=>{const t=S.tx.find(x=>x.id==id);if(t)openForm(t.type,t)};
function saveTx(type,id){const v=k=>($('#'+k)||{}).value,t={id:id||'T'+Date.now().toString(36),date:v('f_d')||today(),type:v('f_ty')||type,amount:+v('f_a'),account:v('f_ac'),to:v('f_to')||'',category:v('f_c')||'',person:(v('f_p')||'').trim(),note:(v('f_n')||'').trim()};
 if(!(t.amount>0))return toast('Enter an amount');
 if(t.type=='TRANSFER'&&t.account==t.to)return toast('Pick two different accounts');
 if(/^(CREDIT|DEBT)/.test(t.type)&&!t.person)return toast('Enter a name');
 if(id)S.tx[S.tx.findIndex(x=>x.id==id)]=t;else S.tx.push(t);
 save();post({action:id?'UPDATE_TRANSACTION':'ADD_TRANSACTION',tx:t});closeM();render();toast('Saved')}
function delTx(id){if(!confirm('Delete this transaction?'))return;S.tx=S.tx.filter(x=>x.id!=id);save();post({action:'DELETE_TRANSACTION',id});closeM();render()}

/* ---------- Google Sheets sync ---------- */
async function post(b){const u=localStorage.api;if(!u)return true;try{const r=await(await fetch(u,{method:'POST',headers:{'Content-Type':'text/plain'},body:JSON.stringify(b)})).json();if(!r.ok){toast('Sheet error: '+(r.error||'unknown'));return false}return true}catch(e){toast('NOT saved to Sheet. Check URL and "Anyone" access');return false}}
async function load(manual){const u=localStorage.api;if(!u)return;
 if(!/\/exec$/.test(u))return toast('URL must end with /exec');
 try{const r=await(await fetch(u)).json();if(!Array.isArray(r.transactions))throw 0;
  if(!r.transactions.length&&S.tx.length&&confirm('Your Sheet is empty. Upload the '+S.tx.length+' entries saved on this device?')){for(const t of S.tx)if(!await post({action:'ADD_TRANSACTION',tx:t}))return}
  else S.tx=r.transactions;
  S.open=r.accounts||S.open;save();render();if(manual)toast('Connected ✓ '+S.tx.length+' entries')}
 catch(e){toast('Cannot reach Sheet. Redeploy: Execute as Me, Access Anyone')}}

/* ---------- theme, PIN, navigation ---------- */
function theme(t){localStorage.th=t;document.documentElement.dataset.t=t=='dark'||(t=='system'&&matchMedia('(prefers-color-scheme:dark)').matches)?'dark':'light'}
async function hash(p){try{const b=await crypto.subtle.digest('SHA-256',new TextEncoder().encode('sm'+p));return[...new Uint8Array(b)].map(x=>x.toString(16).padStart(2,'0')).join('')}catch{return btoa(p)}}
async function setPin(){const p=$('#np').value;if(!/^\d{4}$/.test(p))return toast('PIN must be 4 digits');localStorage.pin=await hash(p);toast('PIN set');render()}
async function unlock(){const p=$('#pin').value;if(p.length==4){if(await hash(p)==localStorage.pin){$('#lock').style.display='none'}$('#pin').value=''}}
const TABS=[['home','🏠','Home'],['tx','💳','Transactions'],['money','💰','Money'],['stats','📊','Stats'],['more','☰','More']];
function render(){$('#app').innerHTML=V[S.tab]();if(S.tab=='tx')showList();$('#fab').style.display=S.tab=='home'?'none':'block';
 $('#nav').innerHTML=TABS.map(t=>`<button class="${S.tab==t[0]?'on':''}" onclick="S.tab='${t[0]}';render();scrollTo(0,0)"><i>${t[1]}</i>${t[2]}</button>`).join('')}
theme(localStorage.th||'system');if(localStorage.pin){$('#lock').style.display='flex'}render();load();
</script></body></html>
