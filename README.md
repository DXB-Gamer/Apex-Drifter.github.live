<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>APEX DRIFT</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Barlow+Condensed:wght@300;400;600;700&display=swap');
:root{--neon:#00ffe7;--hot:#ff3a5c;--gold:#ffc820;--bg:#060810;--panel:rgba(0,255,231,0.06);--border:rgba(0,255,231,0.15);--text:#e8eeff}
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:100%;height:100%;overflow:hidden;background:var(--bg);color:var(--text);font-family:'Barlow Condensed',sans-serif}
canvas#c{display:block;position:fixed;inset:0;width:100%;height:100%}
.screen{position:fixed;inset:0;z-index:50;display:flex;align-items:center;justify-content:center}
.screen.hidden{display:none!important}
#menu{background:radial-gradient(ellipse 80% 60% at 50% 55%,#0c1830 0%,#060810 100%);flex-direction:column}
.menu-grid{display:grid;grid-template-columns:1fr 1fr;width:min(860px,95vw);border:1px solid var(--border);box-shadow:0 0 80px rgba(0,255,231,.07)}
.menu-left{padding:56px 48px;border-right:1px solid var(--border);display:flex;flex-direction:column;justify-content:space-between}
.menu-right{padding:56px 48px;background:var(--panel);display:flex;flex-direction:column;gap:14px;justify-content:center}
.logo{font-family:'Bebas Neue',sans-serif;font-size:clamp(60px,8vw,96px);letter-spacing:5px;line-height:.95;background:linear-gradient(160deg,#fff 20%,var(--neon) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent}
.logo-tag{font-size:11px;letter-spacing:8px;color:var(--neon);opacity:.5;margin-top:6px;text-transform:uppercase}
.logo-wrap{margin-bottom:32px}
.menu-stats{display:flex;flex-direction:column;gap:6px}
.stat-row{display:flex;justify-content:space-between;font-size:13px;padding:8px 0;border-bottom:1px solid rgba(255,255,255,.05)}
.stat-label{color:rgba(255,255,255,.35);letter-spacing:2px;font-size:11px;text-transform:uppercase}
.stat-val{color:var(--neon);font-weight:700;font-size:16px}
.field-label{font-size:10px;letter-spacing:5px;color:var(--neon);opacity:.5;text-transform:uppercase;margin-bottom:6px}
input[type=text]{width:100%;padding:12px 16px;background:rgba(0,0,0,.4);border:1px solid var(--border);color:#fff;font-family:'Barlow Condensed',sans-serif;font-size:17px;font-weight:600;outline:none;transition:border-color .2s;margin-bottom:20px}
input[type=text]:focus{border-color:var(--neon)}
input[type=text]::placeholder{color:rgba(255,255,255,.2)}
.color-row{display:flex;gap:8px;margin-bottom:28px;flex-wrap:wrap}
.cswatch{width:32px;height:32px;border-radius:50%;cursor:pointer;border:3px solid transparent;transition:border-color .15s,transform .15s}
.cswatch.on,.cswatch:hover{border-color:#fff;transform:scale(1.2)}
.btn{width:100%;padding:15px 0;border:none;cursor:pointer;font-family:'Bebas Neue',sans-serif;font-size:21px;letter-spacing:5px;transition:all .18s;display:flex;align-items:center;justify-content:center;gap:12px}
.btn-primary{background:var(--neon);color:#000;box-shadow:0 0 32px rgba(0,255,231,.3);margin-bottom:10px}
.btn-primary:hover{background:#fff;box-shadow:0 0 52px rgba(0,255,231,.5)}
.btn-secondary{background:transparent;color:var(--text);border:1px solid var(--border)}
.btn-secondary:hover{border-color:var(--neon);color:var(--neon)}
.mp-badge{font-size:9px;background:var(--hot);color:#fff;padding:2px 7px;letter-spacing:2px;border-radius:2px;font-family:'Barlow Condensed',sans-serif;font-weight:700}
.btn:active{transform:scale(.97)}
.menu-hint{font-size:11px;color:rgba(255,255,255,.2);letter-spacing:2px;text-align:center;margin-top:8px}
#mp-modal{background:rgba(0,0,0,.85);backdrop-filter:blur(12px)}
.modal-box{width:min(480px,92vw);border:1px solid var(--border);background:#0a0e1a;padding:44px 40px;box-shadow:0 0 80px rgba(0,0,0,.8)}
.modal-title{font-family:'Bebas Neue',sans-serif;font-size:36px;letter-spacing:4px;color:var(--neon);margin-bottom:6px}
.modal-sub{font-size:13px;color:rgba(255,255,255,.35);letter-spacing:1px;margin-bottom:32px;line-height:1.6}
.url-field{width:100%;padding:14px 16px;background:rgba(0,0,0,.6);border:1px solid var(--border);color:var(--neon);font-family:'Barlow Condensed',sans-serif;font-size:14px;outline:none;margin-bottom:8px;transition:border-color .2s;font-weight:600}
.url-field:focus{border-color:var(--neon)}
.url-field::placeholder{color:rgba(0,255,231,.25);font-weight:400}
.url-hint{font-size:11px;color:rgba(255,255,255,.25);letter-spacing:1px;margin-bottom:28px;line-height:1.7}
.url-hint a{color:var(--neon);opacity:.6;text-decoration:none}
.url-hint a:hover{opacity:1}
.modal-btns{display:flex;gap:10px}
.modal-btns .btn{flex:1;padding:14px 0;font-size:18px}
.btn-connect{background:var(--neon);color:#000}
.btn-connect:hover{background:#fff}
.btn-cancel{background:transparent;border:1px solid rgba(255,255,255,.15);color:rgba(255,255,255,.4)}
.btn-cancel:hover{border-color:var(--hot);color:var(--hot)}
.mp-status{margin-top:16px;font-size:12px;letter-spacing:2px;text-align:center;min-height:18px}
.mp-status.err{color:var(--hot)}
.mp-status.ok{color:var(--neon)}
#hud{position:fixed;inset:0;pointer-events:none;z-index:10;display:none}
#speedometer{position:absolute;bottom:30px;right:36px;text-align:right}
#spd{font-family:'Bebas Neue',sans-serif;font-size:80px;line-height:1;color:var(--neon);text-shadow:0 0 30px rgba(0,255,231,.45)}
#spd-unit{font-size:11px;letter-spacing:6px;color:rgba(255,255,255,.3)}
#lap-hud{position:absolute;top:26px;left:50%;transform:translateX(-50%);text-align:center}
#lap-label{font-size:10px;letter-spacing:7px;color:rgba(255,255,255,.3);text-transform:uppercase}
#lap-val{font-family:'Bebas Neue',sans-serif;font-size:46px;color:#fff;line-height:1}
#pos-hud{position:absolute;top:26px;right:36px;text-align:right}
#pos-label{font-size:10px;letter-spacing:6px;color:rgba(255,255,255,.3)}
#pos-val{font-family:'Bebas Neue',sans-serif;font-size:60px;color:var(--gold);line-height:1;text-shadow:0 0 24px rgba(255,200,32,.4)}
#lb{position:absolute;top:26px;left:36px;min-width:190px}
#lb-title{font-size:10px;letter-spacing:6px;color:var(--neon);opacity:.5;margin-bottom:10px}
.lbe{display:flex;align-items:center;gap:10px;padding:5px 0;border-bottom:1px solid rgba(255,255,255,.05);font-size:14px;font-weight:600}
.lbe-pos{width:18px;color:rgba(255,255,255,.25);font-size:11px}
.lbe-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.lbe-name{flex:1}
.lbe-lap{color:rgba(255,255,255,.35);font-size:12px}
#minimap{position:absolute;bottom:30px;left:36px;width:150px;height:150px;border-radius:50%;border:1px solid var(--border);overflow:hidden;background:rgba(0,0,0,.5)}
#mmc{width:100%;height:100%}
#ctrl-hint{position:absolute;bottom:30px;left:50%;transform:translateX(-50%);font-size:10px;letter-spacing:3px;color:rgba(255,255,255,.18);text-transform:uppercase;display:flex;gap:24px}
#drift-hint{position:absolute;bottom:52px;left:50%;transform:translateX(-50%);font-size:10px;letter-spacing:3px;color:rgba(255,200,32,.35);text-transform:uppercase}
#wrong-way{position:fixed;top:42%;left:50%;transform:translate(-50%,-50%);font-family:'Bebas Neue',sans-serif;font-size:68px;color:var(--hot);letter-spacing:8px;text-shadow:0 0 40px rgba(255,58,92,.7);display:none;z-index:20;animation:blink .45s infinite alternate}
@keyframes blink{to{opacity:.2}}
#countdown{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:40;pointer-events:none}
#cdv{font-family:'Bebas Neue',sans-serif;font-size:210px;color:var(--neon);text-shadow:0 0 80px rgba(0,255,231,.55);opacity:0;transform:scale(1.5);transition:opacity .22s,transform .22s}
#cdv.show{opacity:1;transform:scale(1)}
#cdv.go{color:var(--gold);text-shadow:0 0 80px rgba(255,200,32,.7)}
#drift-flash{position:fixed;top:38%;left:50%;transform:translate(-50%,-50%);font-family:'Bebas Neue',sans-serif;font-size:52px;color:var(--gold);letter-spacing:6px;opacity:0;pointer-events:none;z-index:20;transition:opacity .1s}
#mp-dot{position:fixed;top:14px;left:50%;transform:translateX(-50%);font-size:10px;letter-spacing:3px;color:rgba(255,255,255,.2);z-index:30;display:none}
#mp-dot span{display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--neon);box-shadow:0 0 8px var(--neon);margin-right:6px;vertical-align:middle;animation:pulse 1.5s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.3}}
#finish{background:rgba(0,0,0,.8);backdrop-filter:blur(8px);flex-direction:column;display:none}
#finish.show{display:flex}
#finish h1{font-family:'Bebas Neue',sans-serif;font-size:100px;color:var(--gold);text-shadow:0 0 60px rgba(255,200,32,.6);letter-spacing:8px}
#finish p{font-size:20px;color:rgba(255,255,255,.5);margin-top:6px}
#btn-again{margin-top:36px;padding:15px 52px;background:transparent;border:2px solid var(--neon);color:var(--neon);font-family:'Bebas Neue',sans-serif;font-size:22px;letter-spacing:5px;cursor:pointer;transition:all .15s;pointer-events:all}
#btn-again:hover{background:var(--neon);color:#000}
</style>
</head>
<body>
<canvas id="c"></canvas>

<div class="screen" id="menu">
  <div class="menu-grid">
    <div class="menu-left">
      <div class="logo-wrap">
        <div class="logo">APEX<br>DRIFT</div>
        <div class="logo-tag">Racing · 2025</div>
      </div>
      <div class="menu-stats">
        <div class="stat-row"><span class="stat-label">Track</span><span class="stat-val">NEON CIRCUIT</span></div>
        <div class="stat-row"><span class="stat-label">Laps</span><span class="stat-val">3</span></div>
        <div class="stat-row"><span class="stat-label">Drift</span><span class="stat-val">SPACE / SHIFT</span></div>
      </div>
    </div>
    <div class="menu-right">
      <div>
        <div class="field-label">Driver Name</div>
        <input type="text" id="inp-name" placeholder="Enter callsign..." maxlength="16"/>
        <div class="field-label">Car Color</div>
        <div class="color-row" id="color-row"></div>
      </div>
      <div>
        <button class="btn btn-primary" id="btn-sp">▶ &nbsp;SOLO RACE</button>
        <button class="btn btn-secondary" id="btn-mp">⚡ &nbsp;MULTIPLAYER <span class="mp-badge">ONLINE</span></button>
        <div class="menu-hint">WASD / Arrows · SPACE = Drift</div>
      </div>
    </div>
  </div>
</div>

<div class="screen hidden" id="mp-modal">
  <div class="modal-box">
    <div class="modal-title">Multiplayer Setup</div>
    <div class="modal-sub">Enter your Firebase Realtime Database URL to race online.</div>
    <!-- ╔═══════════════════════════════════════════════════╗ -->
    <!-- ║   PASTE YOUR FIREBASE DATABASE URL IN value=""   ║ -->
    <!-- ╚═══════════════════════════════════════════════════╝ -->
    <input class="url-field" id="inp-url" type="text"
      placeholder="https://your-project-default-rtdb.firebaseio.com"
      value="https://racing-game-3c7ef-default-rtdb.firebaseio.com/"/>
    <!-- ↑↑↑  YOUR URL IS ALREADY SET ABOVE  ↑↑↑ -->
    <div class="url-hint">
      Firebase Console → Your Project → Realtime Database<br>
      Make sure Rules allow read/write (test mode is fine).<br>
      <a href="https://console.firebase.google.com" target="_blank">→ Open Firebase Console</a>
    </div>
    <div class="modal-btns">
      <button class="btn btn-connect" id="btn-connect">CONNECT</button>
      <button class="btn btn-cancel" id="btn-cancel">CANCEL</button>
    </div>
    <div class="mp-status" id="mp-status"></div>
  </div>
</div>

<div id="hud">
  <div id="lb"><div id="lb-title">Standings</div><div id="lb-list"></div></div>
  <div id="lap-hud"><div id="lap-label">Lap</div><div id="lap-val">1 / 3</div></div>
  <div id="pos-hud"><div id="pos-label">Position</div><div id="pos-val">P1</div></div>
  <div id="speedometer"><div id="spd">0</div><div id="spd-unit">KM/H</div></div>
  <div id="minimap"><canvas id="mmc" width="150" height="150"></canvas></div>
  <div id="ctrl-hint"><span>W/↑ Accel</span><span>S/↓ Brake</span><span>A D/← → Steer</span><span>SPACE Drift</span></div>
</div>
<div id="mp-dot"><span></span>MULTIPLAYER LIVE</div>
<div id="wrong-way">WRONG WAY</div>
<div id="drift-flash">DRIFT!</div>
<div id="countdown"><div id="cdv"></div></div>
<div class="screen" id="finish">
  <h1 id="fin-title">FINISH!</h1>
  <p id="fin-sub">Race complete</p>
  <button id="btn-again" onclick="location.reload()">RACE AGAIN</button>
</div>

<script async src="https://unpkg.com/es-module-shims@1.8.0/dist/es-module-shims.js"></script>
<script type="importmap">{"imports":{"three":"https://unpkg.com/three@0.158.0/build/three.module.js"}}</script>
<script type="module">
import * as THREE from 'three';

// ══════════════════════════════════════════
//  FIREBASE
// ══════════════════════════════════════════
let db,fbRef,fbSet,fbOnValue,fbOnDisconnect,fbRemove;
let isMultiplayer=false, myId, myFireRef;

// ══════════════════════════════════════════
//  PALETTE
// ══════════════════════════════════════════
const PALETTE=['#00ffe7','#ff3a5c','#ffc820','#a78bfa','#34d399','#f97316','#38bdf8','#fb923c'];
let pickedColor=PALETTE[0];
document.getElementById('color-row') && PALETTE.forEach((c,i)=>{
  const sw=document.createElement('div');
  sw.className='cswatch'+(i===0?' on':'');
  sw.style.background=c;
  sw.onclick=()=>{document.querySelectorAll('.cswatch').forEach(s=>s.classList.remove('on'));sw.classList.add('on');pickedColor=c};
  document.getElementById('color-row').appendChild(sw);
});

document.getElementById('btn-sp').onclick=()=>launch(false);
document.getElementById('inp-name').addEventListener('keydown',e=>{if(e.key==='Enter')launch(false)});
document.getElementById('btn-mp').onclick=()=>{
  document.getElementById('menu').classList.add('hidden');
  document.getElementById('mp-modal').classList.remove('hidden');
};
document.getElementById('btn-cancel').onclick=()=>{
  document.getElementById('mp-modal').classList.add('hidden');
  document.getElementById('menu').classList.remove('hidden');
  document.getElementById('mp-status').textContent='';
};
document.getElementById('btn-connect').onclick=connectMP;

async function connectMP(){
  const url=document.getElementById('inp-url').value.trim();
  const status=document.getElementById('mp-status');
  if(!url||!url.includes('firebaseio.com')){
    status.className='mp-status err';
    status.textContent='✕  Paste a valid Firebase Realtime Database URL';
    return;
  }
  status.className='mp-status';
  status.textContent='Connecting…';
  document.getElementById('btn-connect').disabled=true;
  try{
    const{initializeApp}=await import("https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js");
    const{getDatabase,ref,set,onValue,onDisconnect,remove}=await import("https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js");
    const app=initializeApp({databaseURL:url},'apex-'+Date.now());
    db=getDatabase(app);fbRef=ref;fbSet=set;fbOnValue=onValue;fbOnDisconnect=onDisconnect;fbRemove=remove;
    await Promise.race([
      new Promise(res=>{const u=onValue(ref(db,'.info/connected'),s=>{if(s.val()){u();res()}});}),
      new Promise(res=>setTimeout(res,5000))
    ]);
    status.className='mp-status ok';
    status.textContent='✓  Connected! Starting race…';
    isMultiplayer=true;
  }catch(e){
    status.className='mp-status err';
    status.textContent='⚠  Launching anyway — check DB rules if sync fails';
    console.warn(e);
  }
  setTimeout(()=>launch(true),700);
}

// ══════════════════════════════════════════
//  TRACK
// ══════════════════════════════════════════
const TW=220,TH=115,SEGS=120,RW=22;
const TPTS=[];
for(let i=0;i<=SEGS;i++){
  const t=(i/SEGS)*Math.PI*2;
  let x=TW*Math.cos(t), z=TH*Math.sin(t);
  if(Math.cos(t)>0.5&&Math.sin(t)>0) x+=Math.sin(t*5)*20;
  if(Math.cos(t)<-0.4) z+=Math.cos(t*3)*14;
  TPTS.push(new THREE.Vector3(x,0,z));
}
function trkPt(t){
  const i=((t%1)+1)%1*(TPTS.length-1);
  const lo=Math.floor(i),hi=Math.min(lo+1,TPTS.length-1);
  return TPTS[lo].clone().lerp(TPTS[hi],i-lo);
}
function trkTan(t){
  const dt=0.002;
  return trkPt(t+dt).sub(trkPt(Math.max(t-dt,0))).normalize();
}
const TOTAL_LAPS=3, CP_COUNT=20;
const CPS=[];
for(let i=0;i<CP_COUNT;i++) CPS.push({t:i/CP_COUNT,pos:trkPt(i/CP_COUNT)});

// ══════════════════════════════════════════
//  THREE
// ══════════════════════════════════════════
let scene,camera,renderer;
function initThree(){
  scene=new THREE.Scene();
  scene.background=new THREE.Color(0x080b18);
  scene.fog=new THREE.FogExp2(0x080b18,0.007);
  renderer=new THREE.WebGLRenderer({canvas:document.getElementById('c'),antialias:true});
  renderer.setPixelRatio(Math.min(devicePixelRatio,2));
  renderer.setSize(innerWidth,innerHeight);
  renderer.shadowMap.enabled=true;
  renderer.shadowMap.type=THREE.PCFSoftShadowMap;
  camera=new THREE.PerspectiveCamera(62,innerWidth/innerHeight,0.1,1400);
  scene.add(new THREE.AmbientLight(0x1a2040,1.3));
  const sun=new THREE.DirectionalLight(0xfff0dd,1.5);
  sun.position.set(100,140,80);sun.castShadow=true;
  sun.shadow.mapSize.set(2048,2048);
  sun.shadow.camera.left=-350;sun.shadow.camera.right=350;
  sun.shadow.camera.top=250;sun.shadow.camera.bottom=-250;sun.shadow.camera.far=600;
  scene.add(sun);
  const n1=new THREE.PointLight(0x00ffe7,1.1,130);n1.position.set(-TW,25,0);scene.add(n1);
  const n2=new THREE.PointLight(0xff3a5c,0.9,130);n2.position.set(TW,25,0);scene.add(n2);
  const sv=[];
  for(let i=0;i<4000;i++) sv.push((Math.random()-.5)*2200,Math.random()*500+100,(Math.random()-.5)*2200);
  const sg=new THREE.BufferGeometry();sg.setAttribute('position',new THREE.Float32BufferAttribute(sv,3));
  scene.add(new THREE.Points(sg,new THREE.PointsMaterial({color:0xffffff,size:0.7})));
  window.addEventListener('resize',()=>{camera.aspect=innerWidth/innerHeight;camera.updateProjectionMatrix();renderer.setSize(innerWidth,innerHeight)});
}

// ══════════════════════════════════════════
//  TRACK BUILD
// ══════════════════════════════════════════
function buildTrack(){
  const inner=[],outer=[];
  TPTS.forEach((p,i)=>{
    const t=i/(TPTS.length-1),tan=trkTan(t),n=new THREE.Vector3(-tan.z,0,tan.x);
    inner.push(p.clone().addScaledVector(n,-RW/2));
    outer.push(p.clone().addScaledVector(n,RW/2));
  });
  const verts=[],uvs=[],idx=[];
  for(let i=0;i<inner.length;i++){
    verts.push(inner[i].x,-0.03,inner[i].z,outer[i].x,-0.03,outer[i].z);
    uvs.push(0,i/inner.length,1,i/inner.length);
    if(i<inner.length-1){const b=i*2;idx.push(b,b+1,b+2,b+1,b+3,b+2)}
  }
  const rg=new THREE.BufferGeometry();
  rg.setAttribute('position',new THREE.Float32BufferAttribute(verts,3));
  rg.setAttribute('uv',new THREE.Float32BufferAttribute(uvs,2));
  rg.setIndex(idx);rg.computeVertexNormals();
  const road=new THREE.Mesh(rg,new THREE.MeshStandardMaterial({color:0x171a2c,roughness:.9}));
  road.receiveShadow=true;scene.add(road);

  for(let i=0;i<TPTS.length-1;i+=5){
    const t=i/(TPTS.length-1),p=TPTS[i],tan=trkTan(t);
    const d=new THREE.Mesh(new THREE.PlaneGeometry(0.7,4.5),new THREE.MeshBasicMaterial({color:0xffffff,opacity:.28,transparent:true}));
    d.rotation.x=-Math.PI/2;d.rotation.z=-Math.atan2(tan.x,tan.z);d.position.set(p.x,0.01,p.z);scene.add(d);
  }

  const bR=new THREE.MeshStandardMaterial({color:0xff3a5c,roughness:.6});
  const bW=new THREE.MeshStandardMaterial({color:0xffffff,roughness:.6});
  [inner,outer].forEach(pts=>{
    for(let i=0;i<pts.length-1;i++){
      const a=pts[i],b=pts[i+1],len=a.distanceTo(b);
      const m=new THREE.Mesh(new THREE.BoxGeometry(len+.05,1.3,0.45),Math.floor(i/3)%2===0?bR:bW);
      m.position.copy(a).add(b).multiplyScalar(.5);m.position.y=.65;
      m.rotation.y=Math.atan2(b.x-a.x,b.z-a.z);m.castShadow=true;scene.add(m);
    }
  });

  const sfPos=trkPt(0),sfTan=trkTan(0);
  const sf=new THREE.Mesh(new THREE.PlaneGeometry(RW,1.8),new THREE.MeshBasicMaterial({color:0xffffff,opacity:.9,transparent:true}));
  sf.rotation.x=-Math.PI/2;sf.rotation.z=-Math.atan2(sfTan.x,sfTan.z);sf.position.set(sfPos.x,0.015,sfPos.z);scene.add(sf);

  const ground=new THREE.Mesh(new THREE.PlaneGeometry(1400,1400),new THREE.MeshStandardMaterial({color:0x070a12,roughness:1}));
  ground.rotation.x=-Math.PI/2;ground.position.y=-0.15;ground.receiveShadow=true;scene.add(ground);

  [[-TH-26,70],[TH+26,70]].forEach(([z,w])=>{
    const gs=new THREE.Mesh(new THREE.BoxGeometry(w,14,9),new THREE.MeshStandardMaterial({color:0x111828,roughness:.9}));
    gs.position.set(0,7,z);gs.castShadow=true;scene.add(gs);
  });
  [[-TW-10,0],[TW+10,0],[0,-TH-20],[0,TH+20]].forEach(([x,z])=>{
    const t=new THREE.Mesh(new THREE.CylinderGeometry(.7,1.1,32),new THREE.MeshStandardMaterial({color:0x1a2240,metalness:.5}));
    t.position.set(x,16,z);scene.add(t);
    const sl=new THREE.SpotLight(0xfff5e0,2.2,220,Math.PI/9,.5);
    sl.position.set(x,32,z);sl.target.position.set(x*.25,0,z*.25);scene.add(sl);scene.add(sl.target);
  });
}

// ══════════════════════════════════════════
//  CAR MESH
// ══════════════════════════════════════════
function makeCar(hex){
  const g=new THREE.Group();
  const col=new THREE.Color(hex);
  const body=new THREE.MeshStandardMaterial({color:col,roughness:.28,metalness:.65});
  const dark=new THREE.MeshStandardMaterial({color:0x111111,roughness:.85});
  const glass=new THREE.MeshStandardMaterial({color:0x99ccff,roughness:.05,metalness:.1,transparent:true,opacity:.5});
  const chrome=new THREE.MeshStandardMaterial({color:0xcccccc,metalness:.96,roughness:.04});
  const add=(geo,mat,x,y,z,rx=0,ry=0,rz=0)=>{
    const m=new THREE.Mesh(geo,mat);m.position.set(x,y,z);m.rotation.set(rx,ry,rz);m.castShadow=true;g.add(m);return m;
  };
  add(new THREE.BoxGeometry(4.4,.72,2.1),body,0,.46,0);
  add(new THREE.BoxGeometry(2.1,.62,1.75),body,-.2,1.06,0);
  add(new THREE.BoxGeometry(.1,.55,1.65),glass,.85,1.02,0,0,0,-.24);
  add(new THREE.BoxGeometry(.95,.07,2.15),chrome,-2.02,1.22,0);
  add(new THREE.CylinderGeometry(.07,.07,.55),chrome,-2.02,.98,.72);
  add(new THREE.CylinderGeometry(.07,.07,.55),chrome,-2.02,.98,-.72);
  // wheels — stored so we can spin them
  const wheels=[];
  [[1.38,1],[1.38,-1],[-1.38,1],[-1.38,-1]].forEach(([wx,ws])=>{
    const w=add(new THREE.CylinderGeometry(.44,.44,.3,20),dark,wx,.44,ws*1.1,Math.PI/2,0,0);
    wheels.push(w);
    add(new THREE.CircleGeometry(.24,10),chrome,wx,.44,ws*(1.1+.16),Math.PI/2*(ws>0?1:-1),0,0);
  });
  g.userData.wheels=wheels;
  [[.9,.52,.72],[.9,.52,-.72]].forEach(([x,y,z])=>{
    add(new THREE.BoxGeometry(.14,.18,.32),new THREE.MeshBasicMaterial({color:0xffffee}),x,y,z);
    const pl=new THREE.PointLight(0xffffcc,.9,18);pl.position.set(x+.5,y,z);g.add(pl);
  });
  [[-2.08,.52,.68],[-2.08,.52,-.68]].forEach(([x,y,z])=>{
    add(new THREE.BoxGeometry(.11,.17,.28),new THREE.MeshBasicMaterial({color:0xff1a1a}),x,y,z);
  });
  const glow=new THREE.PointLight(col,1.1,9);glow.position.y=-.15;g.add(glow);
  return g;
}

function makeLabel(text,hex){
  const cv=document.createElement('canvas');cv.width=256;cv.height=64;
  const ctx=cv.getContext('2d');
  ctx.fillStyle=hex;ctx.font='bold 30px "Barlow Condensed",sans-serif';
  ctx.textAlign='center';ctx.fillText(text,128,44);
  const sp=new THREE.Sprite(new THREE.SpriteMaterial({map:new THREE.CanvasTexture(cv),transparent:true,depthTest:false}));
  sp.scale.set(4.5,1.1,1);return sp;
}

// ══════════════════════════════════════════
//  SMOKE PARTICLES
// ══════════════════════════════════════════
const smokeParts=[];
function spawnSmoke(pos){
  const geo=new THREE.SphereGeometry(0.3,4,4);
  const mat=new THREE.MeshBasicMaterial({color:0xaaaaaa,transparent:true,opacity:.4});
  const m=new THREE.Mesh(geo,mat);
  m.position.set(pos.x+(Math.random()-.5)*1.5,0.3,pos.z+(Math.random()-.5)*1.5);
  m._life=1;scene.add(m);smokeParts.push(m);
}
function updateSmoke(){
  for(let i=smokeParts.length-1;i>=0;i--){
    const s=smokeParts[i];s._life-=0.025;
    s.material.opacity=s._life*0.4;
    s.scale.setScalar(1+(1-s._life)*4);
    s.position.y+=0.012;
    if(s._life<=0){scene.remove(s);smokeParts.splice(i,1);}
  }
}

// ══════════════════════════════════════════
//  GAME STATE
// ══════════════════════════════════════════
let carMesh,playerName,playerColor;
const carPos=new THREE.Vector3();
let carAngle=0;  // where car POINTS
let velAngle=0;  // where car MOVES (different = drift)
let carSpd=0;    // scalar speed
let driftAmt=0;
let isDrifting=false;
let lap=1,lastCP=-1,cpTotal=0;
let raceActive=false,raceDone=false,startTime=0;
const keys={};
window.addEventListener('keydown',e=>{keys[e.key]=true;e.preventDefault&&e.key===' '&&e.preventDefault()});
window.addEventListener('keyup',e=>{keys[e.key]=false});
const remotes={};
const camOff=new THREE.Vector3(0,5,-11);
let camCur=new THREE.Vector3();
const mmCtx=document.getElementById('mmc').getContext('2d');

// ══════════════════════════════════════════
//  LAUNCH
// ══════════════════════════════════════════
function launch(mp){
  playerName=document.getElementById('inp-name').value.trim()||'Driver';
  playerColor=pickedColor;
  document.getElementById('menu').classList.add('hidden');
  document.getElementById('mp-modal').classList.add('hidden');
  initThree();buildTrack();
  const sp=trkPt(0.005);sp.y=0.4;carPos.copy(sp);
  const st=trkTan(0.005);
  carAngle=Math.atan2(st.x,st.z);velAngle=carAngle;
  carMesh=makeCar(playerColor);
  carMesh.position.copy(carPos);carMesh.rotation.y=carAngle;
  const lbl=makeLabel(playerName,playerColor);lbl.position.set(0,2.4,0);carMesh.add(lbl);
  scene.add(carMesh);
  document.getElementById('hud').style.display='block';
  document.getElementById('lap-val').textContent=`1 / ${TOTAL_LAPS}`;
  if(mp&&isMultiplayer){setupMP();document.getElementById('mp-dot').style.display='block';}
  countdown(()=>{raceActive=true;startTime=Date.now();});
  requestAnimationFrame(loop);
}

function countdown(cb){
  const el=document.getElementById('cdv');
  const show=(t,c='')=>{el.className='show '+c;el.textContent=t;setTimeout(()=>el.className='',680);};
  show('3');setTimeout(()=>show('2'),1000);setTimeout(()=>show('1'),2000);
  setTimeout(()=>{show('GO!','go');cb();},3000);
}

// ══════════════════════════════════════════
//  PHYSICS — velocity-based drift model
// ══════════════════════════════════════════
const ACCEL=0.016, MAX_SPD=0.72, BRAKE_F=0.024, FRIC=0.980;
const TURN_RATE=0.046, GRIP=0.86, DRIFT_GRIP=0.52;

function aDiff(a,b){let d=a-b;while(d>Math.PI)d-=Math.PI*2;while(d<-Math.PI)d+=Math.PI*2;return d;}

let driftFlashTimer=0;

function physicsUpdate(){
  if(!raceActive||raceDone)return;
  const fwd=keys['ArrowUp']   ||keys['w']||keys['W'];
  const bk =keys['ArrowDown'] ||keys['s']||keys['S'];
  const lft=keys['ArrowLeft'] ||keys['a']||keys['A'];
  const rgt=keys['ArrowRight']||keys['d']||keys['D'];
  const hbk=keys[' ']||keys['Shift']||keys['ShiftLeft']||keys['ShiftRight'];

  // Throttle / brake
  if(fwd)      carSpd=Math.min(carSpd+ACCEL,MAX_SPD);
  else if(bk)  carSpd=Math.max(carSpd-BRAKE_F,0);
  else         carSpd*=FRIC;

  // Steering — scales with speed for feel
  if(carSpd>0.005){
    const tr=TURN_RATE*Math.min(carSpd/0.25,1.0)*(1+driftAmt*0.4);
    if(lft) carAngle+=tr;
    if(rgt) carAngle-=tr;
  }

  // Drift logic
  isDrifting=hbk&&carSpd>0.04;
  const grip=isDrifting?DRIFT_GRIP:GRIP;

  // Blend velocity toward heading
  const diff=aDiff(carAngle,velAngle);
  velAngle+=diff*(1-grip);

  // Drift amount (how sideways we are)
  const curDrift=Math.abs(aDiff(velAngle,carAngle));
  driftAmt+=(curDrift-driftAmt)*0.10;

  // Maintain speed in drift
  if(isDrifting) carSpd=Math.min(carSpd*1.01,MAX_SPD);

  // Move car along velocity vector
  carPos.x+=Math.sin(velAngle)*carSpd;
  carPos.z+=Math.cos(velAngle)*carSpd;
  carPos.y=0.4;

  // ── Visuals ──
  carMesh.position.copy(carPos);

  // Car mesh points heading, rear swings out in drift
  const driftVisual=aDiff(velAngle,carAngle);
  carMesh.rotation.y=carAngle+driftVisual*0.6;

  // Body roll
  const rollT=(lft?0.07:rgt?-0.07:0)*Math.min(carSpd/MAX_SPD*2,1);
  carMesh.rotation.z+=(rollT-carMesh.rotation.z)*0.1;

  // Wheel spin
  if(carMesh.userData.wheels){
    carMesh.userData.wheels.forEach(w=>w.rotation.x+=carSpd*2.4);
  }

  // Smoke on drift
  if((isDrifting||driftAmt>0.08)&&Math.random()<0.5) spawnSmoke(carPos);
  updateSmoke();

  // Drift flash HUD
  const df=document.getElementById('drift-flash');
  if(isDrifting&&carSpd>0.05){
    df.style.opacity='1';
    driftFlashTimer=8;
  } else {
    driftFlashTimer--;
    if(driftFlashTimer<=0) df.style.opacity='0';
  }

  document.getElementById('spd').textContent=Math.round(carSpd*285);
  checkCPs();checkWrongWay();
  if(isMultiplayer) syncFB();
}

// ══════════════════════════════════════════
//  CHECKPOINTS
// ══════════════════════════════════════════
function checkCPs(){
  for(let i=0;i<CPS.length;i++){
    if(carPos.distanceTo(CPS[i].pos)<20&&i!==lastCP){
      if(i===(lastCP+1)%CP_COUNT){
        lastCP=i;cpTotal++;
        if(i===0&&cpTotal>1){
          if(lap<TOTAL_LAPS){lap++;document.getElementById('lap-val').textContent=`${lap} / ${TOTAL_LAPS}`;}
          else finishRace();
        }
      }
    }
  }
}

let wwT=0;
function checkWrongWay(){
  const tan=trkTan(lastCP<0?0:lastCP/CP_COUNT);
  const dir=new THREE.Vector3(Math.sin(carAngle),0,Math.cos(carAngle));
  const ww=document.getElementById('wrong-way');
  if(dir.dot(tan)<-.5&&carSpd>0.04){wwT++;if(wwT>35)ww.style.display='block';}
  else{wwT=0;ww.style.display='none';}
}

function finishRace(){
  raceDone=true;
  const t=((Date.now()-startTime)/1000).toFixed(2);
  document.getElementById('fin-sub').textContent=`Your time: ${t}s`;
  document.getElementById('finish').classList.add('show');
  if(isMultiplayer&&myFireRef) fbRemove(myFireRef);
}

// ══════════════════════════════════════════
//  CAMERA
// ══════════════════════════════════════════
function updateCam(){
  const off=camOff.clone().applyEuler(new THREE.Euler(0,carAngle,0));
  camCur.lerp(carPos.clone().add(off),0.09);
  camera.position.copy(camCur);
  camera.lookAt(carPos.clone().add(new THREE.Vector3(Math.sin(carAngle)*9,1.6,Math.cos(carAngle)*9)));
}

// ══════════════════════════════════════════
//  MINIMAP
// ══════════════════════════════════════════
function drawMM(){
  const W=150,H=150,sx=W/480,sz=H/270,cx=W/2,cz=H/2;
  mmCtx.clearRect(0,0,W,H);
  mmCtx.beginPath();
  TPTS.forEach((p,i)=>{const mx=cx+p.x*sx,mz=cz+p.z*sz;i===0?mmCtx.moveTo(mx,mz):mmCtx.lineTo(mx,mz);});
  mmCtx.closePath();mmCtx.strokeStyle='rgba(255,255,255,.22)';mmCtx.lineWidth=6;mmCtx.stroke();
  Object.values(remotes).forEach(r=>{
    const mx=cx+r.mesh.position.x*sx,mz=cz+r.mesh.position.z*sz;
    mmCtx.beginPath();mmCtx.arc(mx,mz,4,0,Math.PI*2);mmCtx.fillStyle=r.color||'#ff3a5c';mmCtx.fill();
  });
  const mx=cx+carPos.x*sx,mz=cz+carPos.z*sz;
  mmCtx.beginPath();mmCtx.arc(mx,mz,5.5,0,Math.PI*2);mmCtx.fillStyle='#00ffe7';mmCtx.fill();
  mmCtx.beginPath();mmCtx.moveTo(mx,mz);mmCtx.lineTo(mx+Math.sin(carAngle)*10,mz+Math.cos(carAngle)*10);
  mmCtx.strokeStyle='#00ffe7';mmCtx.lineWidth=2;mmCtx.stroke();
}

// ══════════════════════════════════════════
//  MULTIPLAYER
// ══════════════════════════════════════════
function setupMP(){
  myId=Math.random().toString(36).slice(2,10);
  myFireRef=fbRef(db,`apexdrift/players/${myId}`);
  fbOnDisconnect(myFireRef).then(od=>od.remove()).catch(()=>{});
  fbOnValue(fbRef(db,'apexdrift/players'),snap=>{
    const data=snap.val()||{};
    const lb=[];
    Object.entries(data).forEach(([id,p])=>{
      if(id===myId){lb.push({id,name:p.name,color:p.color,lap:p.lap||1,cp:p.cp||0});return;}
      if(!remotes[id]){
        const mesh=makeCar(p.color||'#ff3a5c');
        mesh.position.set(p.x||0,p.y||.4,p.z||0);mesh.rotation.y=p.angle||0;
        const sl=makeLabel(p.name||'Driver',p.color||'#ff3a5c');sl.position.set(0,2.4,0);mesh.add(sl);
        scene.add(mesh);remotes[id]={mesh,color:p.color||'#ff3a5c',lap:1,cp:0};
      } else {
        const r=remotes[id];
        r.mesh.position.lerp(new THREE.Vector3(p.x||0,p.y||.4,p.z||0),.2);
        r.mesh.rotation.y+=aDiff(p.angle||0,r.mesh.rotation.y)*.2;
        r.lap=p.lap||1;r.cp=p.cp||0;
      }
      lb.push({id,name:p.name,color:p.color||'#888',lap:p.lap||1,cp:p.cp||0});
    });
    Object.keys(remotes).forEach(id=>{if(!data[id]){scene.remove(remotes[id].mesh);delete remotes[id];}});
    renderLB(lb);
  });
}

let lastSync=0;
function syncFB(){
  const now=Date.now();if(now-lastSync<50)return;lastSync=now;
  fbSet(myFireRef,{x:carPos.x,y:carPos.y,z:carPos.z,angle:carAngle,lap,cp:cpTotal,name:playerName,color:playerColor});
}

function renderLB(players){
  players.sort((a,b)=>b.lap-a.lap||b.cp-a.cp);
  const r=players.findIndex(p=>p.id===myId)+1;
  document.getElementById('pos-val').textContent=r>0?`P${r}`:'P1';
  const list=document.getElementById('lb-list');list.innerHTML='';
  players.slice(0,6).forEach((p,i)=>{
    const d=document.createElement('div');d.className='lbe';
    d.innerHTML=`<span class="lbe-pos">${i+1}</span><span class="lbe-dot" style="background:${p.color}"></span><span class="lbe-name">${p.name||'Driver'}</span><span class="lbe-lap">L${p.lap}</span>`;
    list.appendChild(d);
  });
}

// ══════════════════════════════════════════
//  LOOP
// ══════════════════════════════════════════
function loop(){
  requestAnimationFrame(loop);
  physicsUpdate();updateCam();drawMM();
  renderer.render(scene,camera);
}
</script>
</body>
</html>
