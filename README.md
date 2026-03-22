<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>APEX DRIFT</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Barlow+Condensed:wght@300;400;600;700&display=swap');
:root{--neon:#00ffe7;--hot:#ff3a5c;--gold:#ffc820;--bg:#060810;--border:rgba(0,255,231,0.15);--text:#e8eeff}
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:100%;height:100%;overflow:hidden;background:var(--bg);color:var(--text);font-family:'Barlow Condensed',sans-serif}
canvas#c{display:block;position:fixed;inset:0;width:100%;height:100%}
.screen{position:fixed;inset:0;z-index:50;display:flex;align-items:center;justify-content:center}
.off{display:none!important}

/* -- MENU -- */
#menu{background:radial-gradient(ellipse 80% 60% at 50% 55%,#0b1628 0%,#060810 100%)}
.card{width:min(460px,92vw);border:1px solid var(--border);background:#080c18;padding:44px 40px;box-shadow:0 0 80px rgba(0,255,231,.05)}
.logo{font-family:'Bebas Neue',sans-serif;font-size:76px;letter-spacing:6px;line-height:.9;background:linear-gradient(160deg,#fff 20%,var(--neon));-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:4px}
.logo-sub{font-size:11px;letter-spacing:8px;color:var(--neon);opacity:.4;text-transform:uppercase;margin-bottom:32px}
.fl{font-size:10px;letter-spacing:5px;color:var(--neon);opacity:.4;text-transform:uppercase;margin-bottom:6px}
input[type=text]{width:100%;padding:11px 14px;background:rgba(0,0,0,.5);border:1px solid var(--border);color:#fff;font-family:'Barlow Condensed',sans-serif;font-size:17px;font-weight:600;outline:none;transition:border-color .2s;margin-bottom:16px}
input[type=text]:focus{border-color:var(--neon)}
input[type=text]::placeholder{color:rgba(255,255,255,.18)}
.swatches{display:flex;gap:8px;margin-bottom:18px}
.sw{width:30px;height:30px;border-radius:50%;cursor:pointer;border:3px solid transparent;transition:all .15s}
.sw.on,.sw:hover{border-color:#fff;transform:scale(1.18)}
/* View toggle */
.view-toggle{display:flex;gap:0;margin-bottom:20px;border:1px solid var(--border);border-radius:3px;overflow:hidden}
.vbtn{flex:1;padding:10px;background:transparent;color:rgba(255,255,255,.4);font-family:'Bebas Neue',sans-serif;font-size:16px;letter-spacing:3px;border:none;cursor:pointer;transition:all .15s}
.vbtn.on{background:var(--neon);color:#000}
.vbtn:not(.on):hover{background:rgba(0,255,231,.1);color:var(--neon)}
.btn-go{width:100%;padding:14px;background:var(--neon);color:#000;font-family:'Bebas Neue',sans-serif;font-size:22px;letter-spacing:6px;border:none;cursor:pointer;transition:all .18s;box-shadow:0 0 28px rgba(0,255,231,.22);margin-bottom:8px}
.btn-go:hover{background:#fff}.btn-go:active{transform:scale(.97)}
.btn-mp{width:100%;padding:11px;background:transparent;color:rgba(255,255,255,.4);font-family:'Bebas Neue',sans-serif;font-size:16px;letter-spacing:5px;border:1px solid rgba(0,255,231,.2);cursor:pointer;transition:all .18s;display:flex;align-items:center;justify-content:center;gap:8px}
.btn-mp:hover{border-color:var(--neon);color:var(--neon)}
.badge{font-size:9px;background:var(--hot);color:#fff;padding:2px 6px;border-radius:2px;font-family:'Barlow Condensed',sans-serif;font-weight:700;letter-spacing:2px}
.hint{margin-top:12px;font-size:10px;color:rgba(255,255,255,.14);letter-spacing:2px;text-align:center}

/* -- MP MODAL -- */
#mp-modal{background:rgba(0,0,0,.9);backdrop-filter:blur(16px)}
.mbox{width:min(400px,92vw);border:1px solid var(--border);background:#090d1c;padding:40px 36px;text-align:center}
.mbox h2{font-family:'Bebas Neue',sans-serif;font-size:28px;letter-spacing:4px;color:var(--neon);margin-bottom:10px}
#mp-status{font-size:13px;letter-spacing:1px;margin-bottom:20px;min-height:18px;color:rgba(255,255,255,.4);line-height:1.6;white-space:pre-line}
#mp-status.ok{color:var(--neon)}#mp-status.err{color:var(--hot)}
.mrow{display:flex;gap:8px}
.mrow button{flex:1;padding:12px;font-family:'Bebas Neue',sans-serif;font-size:16px;letter-spacing:4px;border:none;cursor:pointer;transition:all .15s}
#btn-conn{background:var(--neon);color:#000}#btn-conn:hover{background:#fff}
#btn-conn:disabled{opacity:.35;cursor:not-allowed}
#btn-cxl{background:transparent;border:1px solid rgba(255,255,255,.15);color:rgba(255,255,255,.35)}
#btn-cxl:hover{border-color:var(--hot);color:var(--hot)}

/* -- PAUSE -- */
#pause{background:rgba(0,0,0,.78);backdrop-filter:blur(10px);flex-direction:column;gap:16px}
#pause h2{font-family:'Bebas Neue',sans-serif;font-size:72px;letter-spacing:6px;color:#fff}
#pause button{padding:13px 48px;font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:5px;border:none;cursor:pointer;transition:all .15s}
#btn-resume{background:var(--neon);color:#000}#btn-resume:hover{background:#fff}
#btn-quit{background:transparent;border:1px solid rgba(255,255,255,.2);color:rgba(255,255,255,.45)}
#btn-quit:hover{border-color:var(--hot);color:var(--hot)}

/* -- HUD -- */
#hud{position:fixed;inset:0;pointer-events:none;z-index:10;display:none}
#lap-hud{position:absolute;top:18px;left:50%;transform:translateX(-50%);text-align:center}
#lap-label{font-size:10px;letter-spacing:7px;color:rgba(255,255,255,.28);text-transform:uppercase}
#lap-val{font-family:'Bebas Neue',sans-serif;font-size:40px;color:#fff;line-height:1}
#lb{position:absolute;top:18px;left:22px;min-width:155px}
#lb-title{font-size:10px;letter-spacing:5px;color:var(--neon);opacity:.35;margin-bottom:7px}
.lbe{display:flex;align-items:center;gap:7px;padding:4px 0;border-bottom:1px solid rgba(255,255,255,.05);font-size:12px;font-weight:600}
.lbe-pos{width:14px;color:rgba(255,255,255,.22);font-size:10px}.lbe-dot{width:7px;height:7px;border-radius:50%;flex-shrink:0}.lbe-name{flex:1}.lbe-lap{color:rgba(255,255,255,.3);font-size:10px}
#gear-hud{position:absolute;top:18px;right:22px;text-align:right}
#gear-label{font-size:10px;letter-spacing:5px;color:rgba(255,255,255,.28);text-transform:uppercase}
#gear-val{font-family:'Bebas Neue',sans-serif;font-size:52px;color:var(--gold);line-height:1}
#minimap{position:absolute;bottom:160px;left:22px;width:120px;height:120px;border-radius:50%;border:1px solid var(--border);overflow:hidden;background:rgba(0,0,0,.5)}
#mmc{width:100%;height:100%}
/* speedometer bottom right */
#speedo{position:absolute;bottom:22px;right:22px;text-align:right}
#spd{font-family:'Bebas Neue',sans-serif;font-size:64px;line-height:1;color:var(--neon);text-shadow:0 0 20px rgba(0,255,231,.4)}
#spd-unit{font-size:9px;letter-spacing:5px;color:rgba(255,255,255,.25)}
#rpm-wrap{position:absolute;bottom:98px;right:22px;width:220px;height:4px;background:rgba(255,255,255,.07);border-radius:2px;overflow:hidden}
#rpm-bar{height:100%;width:0%;border-radius:2px;transition:width .04s}
#rpm-label{position:absolute;bottom:108px;right:22px;font-size:11px;letter-spacing:3px;color:rgba(255,255,255,.22);font-family:'Bebas Neue',sans-serif}
/* cockpit overlay (FPS only) */
#cockpit{position:absolute;bottom:0;left:0;right:0;height:170px;pointer-events:none;display:none}
#wheel-wrap{position:absolute;bottom:8px;left:50%;transform:translateX(-50%);width:160px;height:160px;filter:drop-shadow(0 0 12px rgba(0,255,231,.14))}
#wheel-svg{width:100%;height:100%}
/* MP badge */
#mp-dot{position:fixed;top:10px;left:50%;transform:translateX(-50%);font-size:10px;letter-spacing:3px;color:rgba(255,255,255,.18);z-index:30;display:none;pointer-events:none}
#mp-dot span{display:inline-block;width:6px;height:6px;border-radius:50%;background:var(--neon);box-shadow:0 0 8px var(--neon);margin-right:5px;vertical-align:middle;animation:pulse 1.4s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.2}}
/* ESC hint */
#esc-hint{position:fixed;bottom:18px;left:50%;transform:translateX(-50%);font-size:10px;letter-spacing:4px;color:rgba(255,255,255,.16);pointer-events:none;z-index:15}
/* overlays */
#gear-flash{position:fixed;top:33%;left:50%;transform:translate(-50%,-50%);font-family:'Bebas Neue',sans-serif;font-size:68px;letter-spacing:4px;opacity:0;pointer-events:none;z-index:25;transition:opacity .06s}
#wrong-way{position:fixed;top:40%;left:50%;transform:translate(-50%,-50%);font-family:'Bebas Neue',sans-serif;font-size:62px;color:var(--hot);letter-spacing:8px;text-shadow:0 0 40px rgba(255,58,92,.7);display:none;z-index:20;animation:blink .45s infinite alternate}
@keyframes blink{to{opacity:.2}}
#cdv-wrap{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:40;pointer-events:none}
#cdv{font-family:'Bebas Neue',sans-serif;font-size:200px;color:var(--neon);text-shadow:0 0 80px rgba(0,255,231,.55);opacity:0;transform:scale(1.5);transition:opacity .22s,transform .22s}
#cdv.show{opacity:1;transform:scale(1)}#cdv.go{color:var(--gold);text-shadow:0 0 80px rgba(255,200,32,.7)}
#vignette{position:fixed;inset:0;pointer-events:none;z-index:5;background:radial-gradient(ellipse at center,transparent 52%,rgba(0,0,0,.65) 100%)}
#hit-flash{position:fixed;inset:0;pointer-events:none;z-index:15;opacity:0;background:rgba(255,58,92,.22);transition:opacity .05s}
#finish{background:rgba(0,0,0,.85);backdrop-filter:blur(10px);flex-direction:column;display:none}
#finish.show{display:flex}
#finish h1{font-family:'Bebas Neue',sans-serif;font-size:92px;color:var(--gold);text-shadow:0 0 60px rgba(255,200,32,.6);letter-spacing:8px}
#finish p{font-size:18px;color:rgba(255,255,255,.4);margin-top:6px}
#btn-again{margin-top:32px;padding:13px 48px;background:transparent;border:2px solid var(--neon);color:var(--neon);font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:5px;cursor:pointer;transition:all .15s;pointer-events:all}
#btn-again:hover{background:var(--neon);color:#000}
</style>
</head>
<body>
<canvas id="c"></canvas>
<div id="vignette"></div>
<div id="hit-flash"></div>
<div id="gear-flash"></div>

<!-- MENU -->
<div class="screen" id="menu">
  <div class="card">
    <div class="logo">APEX<br>DRIFT</div>
    <div class="logo-sub">Multiplayer Racing</div>
    <div class="fl">Driver Name</div>
    <input type="text" id="inp-name" placeholder="Enter callsign..." maxlength="16"/>
    <div class="fl">Car Color</div>
    <div class="swatches" id="swatches"></div>
    <div class="fl">Camera View</div>
    <div class="view-toggle">
      <button class="vbtn on" id="vbtn-3p">3RD PERSON</button>
      <button class="vbtn" id="vbtn-1p">1ST PERSON</button>
    </div>
    <button class="btn-go" id="btn-go">  &nbsp;SOLO RACE</button>
    <button class="btn-mp" id="btn-mp">  &nbsp;MULTIPLAYER &nbsp;<span class="badge">ONLINE</span></button>
    <div class="hint">W=Gas   S=Brake   A/D=Steer     =Gear   Space=Drift   ESC=Pause</div>
  </div>
</div>

<!-- MP MODAL -->
<div class="screen off" id="mp-modal">
  <div class="mbox">
    <h2>Multiplayer</h2>
    <div id="mp-status">Connecting to race server </div>
    <div class="mrow">
      <button id="btn-conn" disabled>JOIN RACE</button>
      <button id="btn-cxl">CANCEL</button>
    </div>
  </div>
</div>

<!-- PAUSE -->
<div class="screen off" id="pause">
  <h2>PAUSED</h2>
  <button id="btn-resume">RESUME</button>
  <button id="btn-quit" onclick="location.reload()">QUIT TO MENU</button>
</div>

<!-- HUD -->
<div id="hud">
  <div id="lb"><div id="lb-title">Standings</div><div id="lb-list"></div></div>
  <div id="lap-hud"><div id="lap-label">Lap</div><div id="lap-val">1 / 3</div></div>
  <div id="gear-hud"><div id="gear-label">Gear</div><div id="gear-val">1</div></div>
  <div id="minimap"><canvas id="mmc" width="120" height="120"></canvas></div>
  <div id="speedo"><div id="spd">0</div><div id="spd-unit">KM/H</div></div>
  <div id="fps-display" style="position:absolute;top:18px;left:50%;margin-left:80px;font-family:'Bebas Neue',sans-serif;font-size:18px;letter-spacing:3px;color:rgba(0,255,231,0.35)">-- FPS</div>
  <div id="rpm-label">RPM</div>
  <div id="rpm-wrap"><div id="rpm-bar"></div></div>
  <!-- FPS cockpit overlay (shown only in 1st person) -->
  <div id="cockpit">
    <div id="wheel-wrap">
      <svg id="wheel-svg" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <circle cx="100" cy="100" r="88" fill="none" stroke="#12172a" stroke-width="18"/>
        <circle cx="100" cy="100" r="88" fill="none" stroke="url(#rg)" stroke-width="9" stroke-dasharray="5 3"/>
        <circle cx="100" cy="100" r="26" fill="#0d1120" stroke="#00ffe7" stroke-width="1.5" opacity=".85"/>
        <line x1="100" y1="74" x2="100" y2="14" stroke="#1e2340" stroke-width="11" stroke-linecap="round"/>
        <line x1="100" y1="74" x2="100" y2="14" stroke="#2d3260" stroke-width="5" stroke-linecap="round"/>
        <line x1="100" y1="126" x2="55" y2="188" stroke="#1e2340" stroke-width="11" stroke-linecap="round"/>
        <line x1="100" y1="126" x2="55" y2="188" stroke="#2d3260" stroke-width="5" stroke-linecap="round"/>
        <line x1="100" y1="126" x2="145" y2="188" stroke="#1e2340" stroke-width="11" stroke-linecap="round"/>
        <line x1="100" y1="126" x2="145" y2="188" stroke="#2d3260" stroke-width="5" stroke-linecap="round"/>
        <circle cx="100" cy="100" r="5" fill="#00ffe7" opacity=".7"/>
        <rect x="96" y="9" width="8" height="13" rx="4" fill="#00ffe7" opacity=".6"/>
        <defs><linearGradient id="rg" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" style="stop-color:#00ffe7;stop-opacity:.55"/>
          <stop offset="100%" style="stop-color:#00ffe7;stop-opacity:.55"/>
        </linearGradient></defs>
      </svg>
    </div>
  </div>
</div>

<div id="mp-dot"><span></span>ONLINE</div>
<div id="esc-hint">ESC = PAUSE</div>
<div id="wrong-way">WRONG WAY</div>
<div id="cdv-wrap"><div id="cdv"></div></div>
<div class="screen" id="finish">
  <h1>FINISH!</h1><p id="fin-sub"></p>
  <button id="btn-again" onclick="location.reload()">RACE AGAIN</button>
</div>

<script async src="https://unpkg.com/es-module-shims@1.8.0/dist/es-module-shims.js"></script>
<script type="importmap">{"imports":{"three":"https://unpkg.com/three@0.158.0/build/three.module.js"}}</script>
<script type="module">
import * as THREE from 'three';

// ==============================================
//  FIREBASE REST   no SDK, works everywhere
//     PASTE YOUR FIREBASE URL HERE   
// ==============================================
const FB = 'https://apex-drifter-official-default-rtdb.firebaseio.com';

let isMP=false, myId='', lastSync=0;
const remotes={};

async function fbWrite(data){
  try{ await fetch(`${FB}/apexdrift/players/${myId}.json`,{method:'PUT',headers:{'Content-Type':'application/json'},body:JSON.stringify(data)}); }catch(e){}
}
async function fbRemoveMe(){
  try{ await fetch(`${FB}/apexdrift/players/${myId}.json`,{method:'DELETE'}); }catch(e){}
  // Also try sendBeacon as backup (works even during page unload)
  try{ navigator.sendBeacon(`${FB}/apexdrift/players/${myId}.json`, ''); }catch(e){}
}
let pollTimer=null;
function startPolling(){
  pollTimer=setInterval(async()=>{
    try{
      const res=await fetch(`${FB}/apexdrift/players.json`);
      if(!res.ok)return;
      const data=await res.json();
      if(data) handleRemotes(data);
    }catch(e){}
  },30);
}
function stopPolling(){ clearInterval(pollTimer); }

// ==============================================
//  PALETTE & VIEW CHOICE
// ==============================================
const PALETTE=['#00ffe7','#ff3a5c','#ffc820','#a78bfa','#34d399','#f97316','#38bdf8','#fb923c'];
let pickedColor=PALETTE[0];
let viewMode='3p'; // '3p' or '1p'

PALETTE.forEach((c,i)=>{
  const sw=document.createElement('div');
  sw.className='sw'+(i===0?' on':''); sw.style.background=c;
  sw.onclick=()=>{ document.querySelectorAll('.sw').forEach(s=>s.classList.remove('on')); sw.classList.add('on'); pickedColor=c; };
  document.getElementById('swatches').appendChild(sw);
});

document.getElementById('vbtn-3p').onclick=()=>{ viewMode='3p'; document.getElementById('vbtn-3p').classList.add('on'); document.getElementById('vbtn-1p').classList.remove('on'); };
document.getElementById('vbtn-1p').onclick=()=>{ viewMode='1p'; document.getElementById('vbtn-1p').classList.add('on'); document.getElementById('vbtn-3p').classList.remove('on'); };

// ==============================================
//  TRACK
// ==============================================
const TW=200,TH=90,SEGS=200,RW=26;
const TPTS=[];
for(let i=0;i<=SEGS;i++){
  const t=(i/SEGS)*Math.PI*2;
  TPTS.push(new THREE.Vector3(TW*Math.cos(t),0,TH*Math.sin(t)));
}
function trkPt(u){
  const idx=((u%1)+1)%1*(TPTS.length-1),lo=Math.floor(idx),hi=Math.min(lo+1,TPTS.length-1);
  return TPTS[lo].clone().lerp(TPTS[hi],idx-lo);
}
function trkTan(u){ const dt=.001; return trkPt(u+dt).sub(trkPt(((u-dt)%1+1)%1)).normalize(); }
function trkNorm(u){ const t=trkTan(u); return new THREE.Vector3(-t.z,0,t.x); }

const CP_COUNT=24,TOTAL_LAPS=3;
const CPS=[];
for(let i=0;i<CP_COUNT;i++) CPS.push({pos:trkPt(i/CP_COUNT)});

// Wall collision segments
const innerEdge=[],outerEdge=[];
for(let i=0;i<=SEGS;i++){
  const u=i/SEGS,n=trkNorm(u),p=trkPt(u);
  innerEdge.push({x:p.x-n.x*(RW/2),z:p.z-n.z*(RW/2)});
  outerEdge.push({x:p.x+n.x*(RW/2),z:p.z+n.z*(RW/2)});
}
function buildWalls(pts){
  const s=[];
  for(let i=0;i<pts.length-1;i++){
    const a=pts[i],b=pts[i+1],dx=b.x-a.x,dz=b.z-a.z,len=Math.sqrt(dx*dx+dz*dz);
    if(len>.01) s.push({ax:a.x,az:a.z,bx:b.x,bz:b.z,len});
  }
  return s;
}
let wallsI=[],wallsO=[];

// ==============================================
//  THREE.JS
// ==============================================
let scene,camera,renderer;
function initThree(){
  scene=new THREE.Scene();
  scene.background=new THREE.Color(0x080b18);
  scene.fog=new THREE.FogExp2(0x080b18,.0055);
  renderer=new THREE.WebGLRenderer({canvas:document.getElementById('c'),antialias:true,powerPreference:'high-performance',precision:'highp'});
  renderer.setPixelRatio(devicePixelRatio);
  renderer.setSize(innerWidth,innerHeight);
  renderer.shadowMap.enabled=false;
  
  camera=new THREE.PerspectiveCamera(70,innerWidth/innerHeight,.1,1400);
  scene.add(new THREE.AmbientLight(0x1a2040,1.4));
  const sun=new THREE.DirectionalLight(0xfff0dd,1.5);
  sun.position.set(100,140,80);sun.castShadow=true;
  sun.shadow.mapSize.set(1024,1024);
  sun.shadow.camera.left=-350;sun.shadow.camera.right=350;
  sun.shadow.camera.top=200;sun.shadow.camera.bottom=-200;sun.shadow.camera.far=600;
  scene.add(sun);
  const nl1=new THREE.PointLight(0x00ffe7,1.1,160); nl1.position.set(-TW,20,0); scene.add(nl1);
  const nl2=new THREE.PointLight(0xff3a5c,.9,160);  nl2.position.set(TW,20,0);  scene.add(nl2);
  const sv=[];for(let i=0;i<1500;i++)sv.push((Math.random()-.5)*2200,Math.random()*500+80,(Math.random()-.5)*2200);
  const sg=new THREE.BufferGeometry();sg.setAttribute('position',new THREE.Float32BufferAttribute(sv,3));
  scene.add(new THREE.Points(sg,new THREE.PointsMaterial({color:0xffffff,size:.7})));
  window.addEventListener('resize',()=>{camera.aspect=innerWidth/innerHeight;camera.updateProjectionMatrix();renderer.setSize(innerWidth,innerHeight);});
}

function buildTrack(){
  const inner=[],outer=[];
  for(let i=0;i<=SEGS;i++){
    const u=i/SEGS,n=trkNorm(u),p=trkPt(u);
    inner.push(p.clone().addScaledVector(n,-RW/2));
    outer.push(p.clone().addScaledVector(n,RW/2));
  }
  // Road surface
  const rv=[],ruv=[],ri=[];
  for(let i=0;i<inner.length;i++){
    rv.push(inner[i].x,0,inner[i].z,outer[i].x,0,outer[i].z);
    ruv.push(0,i/inner.length,1,i/inner.length);
    if(i<inner.length-1){const b=i*2;ri.push(b,b+1,b+2,b+1,b+3,b+2);}
  }
  const rg=new THREE.BufferGeometry();
  rg.setAttribute('position',new THREE.Float32BufferAttribute(rv,3));
  rg.setAttribute('uv',new THREE.Float32BufferAttribute(ruv,2));
  rg.setIndex(ri);rg.computeVertexNormals();
  const road=new THREE.Mesh(rg,new THREE.MeshStandardMaterial({color:0x181b2e,roughness:.92}));
  road.receiveShadow=true;scene.add(road);
  // Dashes
  for(let i=0;i<SEGS;i+=4){
    const u=i/SEGS,p=trkPt(u),tan=trkTan(u);
    const d=new THREE.Mesh(new THREE.PlaneGeometry(.5,3.5),new THREE.MeshBasicMaterial({color:0xffffff,opacity:.18,transparent:true}));
    d.rotation.x=-Math.PI/2;d.rotation.z=-Math.atan2(tan.x,tan.z);d.position.set(p.x,.01,p.z);scene.add(d);
  }
  // Walls
  const mR=new THREE.MeshStandardMaterial({color:0xff2244,roughness:.55});
  const mW=new THREE.MeshStandardMaterial({color:0xffffff,roughness:.55});
  [inner,outer].forEach(pts=>{
    for(let i=0;i<pts.length-1;i++){
      const a=pts[i],b=pts[i+1],len=a.distanceTo(b);if(len<.01)continue;
      const wall=new THREE.Mesh(new THREE.BoxGeometry(len,.9,.1),Math.floor(i/3)%2===0?mR:mW);
      wall.position.set((a.x+b.x)/2,.65,(a.z+b.z)/2);
      wall.rotation.y=Math.atan2(b.x-a.x,b.z-a.z);scene.add(wall);
      
    }
  });
  wallsI=buildWalls(innerEdge);wallsO=buildWalls(outerEdge);
  // Start line
  const sp=trkPt(0),st=trkTan(0);
  const sf=new THREE.Mesh(new THREE.PlaneGeometry(RW,1.6),new THREE.MeshBasicMaterial({color:0xffffff,opacity:.85,transparent:true}));
  sf.rotation.x=-Math.PI/2;sf.rotation.z=-Math.atan2(st.x,st.z);sf.position.set(sp.x,.015,sp.z);scene.add(sf);
  // Ground
  const gnd=new THREE.Mesh(new THREE.PlaneGeometry(1600,1600),new THREE.MeshStandardMaterial({color:0x060910,roughness:1}));
  gnd.rotation.x=-Math.PI/2;gnd.position.y=-.1;gnd.receiveShadow=true;scene.add(gnd);
  // Stands & towers
  [[-TH-28,80],[TH+28,80]].forEach(([z,w])=>{
    const gs=new THREE.Mesh(new THREE.BoxGeometry(w,16,10),new THREE.MeshStandardMaterial({color:0x101520,roughness:.9}));
    gs.position.set(0,8,z);scene.add(gs);
  });
  [[-TW-12,0],[TW+12,0],[0,-TH-22],[0,TH+22]].forEach(([x,z])=>{
    const tw=new THREE.Mesh(new THREE.CylinderGeometry(.6,1,34),new THREE.MeshStandardMaterial({color:0x1a2240,metalness:.5}));
    tw.position.set(x,17,z);scene.add(tw);
    const sl=new THREE.SpotLight(0xfff5e0,2,240,Math.PI/9,.5);
    sl.position.set(x,34,z);sl.target.position.set(x*.3,0,z*.3);scene.add(sl);scene.add(sl.target);
  });
}

// ==============================================
//  CAR MESH BUILDER
// ==============================================
function buildCarMesh(hex){
  const g=new THREE.Group();
  const col=new THREE.Color(hex);
  const bm=new THREE.MeshStandardMaterial({color:col,roughness:.28,metalness:.65});
  const dm=new THREE.MeshStandardMaterial({color:0x0d0d0d,roughness:.85});
  const gm=new THREE.MeshStandardMaterial({color:0x99ccff,roughness:.05,transparent:true,opacity:.5});
  const cm=new THREE.MeshStandardMaterial({color:0xcccccc,metalness:.95,roughness:.05});
  const add=(geo,mat,x,y,z,ry=0)=>{const m=new THREE.Mesh(geo,mat);m.position.set(x,y,z);m.rotation.y=ry;m.castShadow=true;g.add(m);return m;};
  // Body   long axis along Z (forward)
  add(new THREE.BoxGeometry(2.1,.7,4.4), bm, 0,.46,0);
  add(new THREE.BoxGeometry(1.75,.6,2.1), bm, 0,1.05,-.2);
  add(new THREE.BoxGeometry(1.65,.06,.95), cm, 0,1.22,2.0); // spoiler
  add(new THREE.BoxGeometry(.55,.06,.07), cm, .7,.98,2.0);
  add(new THREE.BoxGeometry(.55,.06,.07), cm,-.7,.98,2.0);
  add(new THREE.BoxGeometry(1.6,.55,.1), gm, 0,1.02,-.85); // windshield
  // Wheels   cylinders on Z axis = Math.PI/2 rotation on Z
  // wheels: [side X, Z position, side sign]
  [[1.08,1.4],[1.08,-1.4],[-1.08,1.4],[-1.08,-1.4]].forEach(([wx,wz])=>{
    const w=new THREE.Mesh(new THREE.CylinderGeometry(.44,.44,.3,18),dm);
    w.rotation.x=Math.PI/2;
    w.position.set(wx,.44,wz);
    w.castShadow=true;g.add(w);
    const hc=new THREE.Mesh(new THREE.CircleGeometry(.24,10),cm);
    hc.rotation.y=wx>0?Math.PI/2:-Math.PI/2;
    hc.position.set(wx+(wx>0?.16:-.16),.44,wz);
    g.add(hc);
  });
  // Lights
  [[.72,.52,2.1],[-.72,.52,2.1]].forEach(([x,y,z])=>{
    add(new THREE.BoxGeometry(.32,.18,.14),new THREE.MeshBasicMaterial({color:0xffffff}),x,y,z);
    const pl=new THREE.PointLight(0xfff5e0, 3.5, 40); // brighter, longer range
    pl.position.set(x,y,z+1); g.add(pl);
    // Spotlight beam for dramatic effect
    const sl=new THREE.SpotLight(0xfff5e0, 8, 60, Math.PI/7, 0.3);
    sl.position.set(x,y,z+0.5);
    sl.target.position.set(x*0.5, -0.5, z+30);
    g.add(sl); g.add(sl.target);
  });
  // Brake/tail lights (always red, brighter when braking)
  const tlMat = new THREE.MeshBasicMaterial({color:0xff2222});
  const brakeGlow1 = new THREE.PointLight(0xff0000, 0.8, 8); brakeGlow1.position.set( .68,.52,-2.5); g.add(brakeGlow1);
  const brakeGlow2 = new THREE.PointLight(0xff0000, 0.8, 8); brakeGlow2.position.set(-.68,.52,-2.5); g.add(brakeGlow2);
  g.userData.brakeGlows = [brakeGlow1, brakeGlow2];
  const tlMat2 = tlMat;
  [[ .68,.52,-2.1],[-.68,.52,-2.1]].forEach(([x,y,z])=>{
    add(new THREE.BoxGeometry(.28,.17,.12), tlMat, x,y,z);
  });
  // Reverse lights (white, shown when reversing)
  const revMat = new THREE.MeshBasicMaterial({color:0xffffff, transparent:true, opacity:0});
  const revLights = [];
  [[ .38,.48,-2.12],[-.38,.48,-2.12]].forEach(([x,y,z])=>{
    const m = new THREE.Mesh(new THREE.BoxGeometry(.22,.14,.1), revMat.clone());
    m.position.set(x,y,z); m.castShadow=false; g.add(m);
    revLights.push(m);
  });
  // Reverse point lights
  const revGlow1 = new THREE.PointLight(0xffffff, 0, 6); revGlow1.position.set( .38,.48,-2.5); g.add(revGlow1);
  const revGlow2 = new THREE.PointLight(0xffffff, 0, 6); revGlow2.position.set(-.38,.48,-2.5); g.add(revGlow2);
  g.userData.revLights = revLights;
  g.userData.revGlows  = [revGlow1, revGlow2];
  const glow=new THREE.PointLight(col, 2.5, 14);glow.position.y=-.15;g.add(glow);
  return g;
}

function makeLabel(name,hex){
  const cv=document.createElement('canvas');cv.width=256;cv.height=56;
  const ctx=cv.getContext('2d');
  ctx.fillStyle=hex;ctx.font='bold 26px sans-serif';ctx.textAlign='center';ctx.fillText(name,128,38);
  const sp=new THREE.Sprite(new THREE.SpriteMaterial({map:new THREE.CanvasTexture(cv),transparent:true,depthTest:false}));
  sp.scale.set(4,1,1);return sp;
}

// ==============================================
//  WALL COLLISION
// ==============================================
// == CAR vs CAR COLLISION ==
const CAR_COL_R = 2.8;
function resolveCarCollisions(){
  if(!isMP) return;
  Object.values(remotes).forEach(r=>{
    const dx=carPos.x-r.mesh.position.x;
    const dz=carPos.z-r.mesh.position.z;
    const dist=Math.sqrt(dx*dx+dz*dz);
    if(dist<CAR_COL_R&&dist>0.01){
      const overlap=CAR_COL_R-dist;
      const nx=dx/dist, nz=dz/dist;
      carPos.x+=nx*overlap*0.6;
      carPos.z+=nz*overlap*0.6;
      carSpdMS=Math.max(0,carSpdMS*(1-overlap/CAR_COL_R*0.5));
      carSpd=carSpdMS/277.8;
      const fl=document.getElementById('hit-flash');
      fl.style.opacity='1';setTimeout(()=>fl.style.opacity='0',80);
    }
  });
}

const CAR_R=1.9, WALL_BOUNCE=0.3;
function resolveWalls(){
  let hit=false;
  for(const w of [...wallsI,...wallsO]){
    const ex=carPos.x-w.ax,ez=carPos.z-w.az;
    const wdx=w.bx-w.ax,wdz=w.bz-w.az;
    const wux=wdx/w.len,wuz=wdz/w.len;
    const t=ex*wux+ez*wuz;
    if(t<0||t>w.len)continue;
    const cpx=w.ax+wux*t,cpz=w.az+wuz*t;
    const dx=carPos.x-cpx,dz=carPos.z-cpz;
    const dist=Math.sqrt(dx*dx+dz*dz);
    if(dist<CAR_R&&dist>.001){
      carPos.x+=dx/dist*(CAR_R-dist);
      carPos.z+=dz/dist*(CAR_R-dist);
      carSpdMS*=WALL_BOUNCE;carSpd=carSpdMS/277.8;
      hit=true;
    }
  }
  if(hit){const fl=document.getElementById('hit-flash');fl.style.opacity='1';setTimeout(()=>fl.style.opacity='0',80);}
}

// ==============================================
//  SMOKE
// ==============================================
const smoke=[];
function spawnSmoke(pos){
  const m=new THREE.Mesh(new THREE.SphereGeometry(.28,4,4),new THREE.MeshBasicMaterial({color:0x888,transparent:true,opacity:.38}));
  m.position.set(pos.x+(Math.random()-.5)*1.4,.28,pos.z+(Math.random()-.5)*1.4);m._life=1;scene.add(m);smoke.push(m);
}
function tickSmoke(){
  for(let i=smoke.length-1;i>=0;i--){
    const s=smoke[i];s._life-=.024;s.material.opacity=s._life*.38;s.scale.setScalar(1+(1-s._life)*4);s.position.y+=.011;
    if(s._life<=0){scene.remove(s);smoke.splice(i,1);}
  }
}

// ==============================================
//  ENGINE   12 GEARS, REAL PHYSICS
// ==============================================
// Arcade gear system - simple and always works
const GEAR_TOP_MS=[22, 60, 120, 180, 240, 277.8]; // G6=1000km/h!
const GEAR_ACC=[45, 38, 30, 22, 16, 11]; // punch off the line
// Gear ratios   12 gears, top speed ~500 km/h on G12
const GEARS=[3.82,2.36,1.69,1.31,1.04,0.87,0.76,0.67,0.60,0.54,0.49,0.45];
const RPM_IDLE=900,RPM_LIMIT=7200,RPM_PEAK=4200,MAX_TQ=680;
let gear=0,rpm=RPM_IDLE,clutch=0,shiftTimer=0;
let carSpdMS=0,carSpd=0;

function shiftGear(d){
  const next=Math.max(0,Math.min(5,gear+d));
  if(next===gear)return;
  gear=next;clutch=1;shiftTimer=SHIFT_MS;
  document.getElementById('gear-val').textContent=gear+1;
  const gf=document.getElementById('gear-flash');
  gf.textContent=(d>0?'  ':'  ')+(gear+1);
  gf.style.color=d>0?'var(--neon)':'var(--gold)';
  gf.style.opacity='1';setTimeout(()=>gf.style.opacity='0',380);
}

// ==============================================
//  INPUT
// ==============================================
const keys={};
window.addEventListener('keydown',e=>{
  keys[e.key]=true;
  if(e.key===' ')e.preventDefault();
  if(e.key==='Escape'){
    if(!gameOn&&!done)return;
    paused=!paused;
    document.getElementById('pause').classList.toggle('off',!paused);
  }
  if(gameOn&&!done&&!paused){
    if(e.key==='ArrowUp')  shiftGear(1);
    if(e.key==='ArrowDown')shiftGear(-1);
  }
});
window.addEventListener('keyup',e=>keys[e.key]=false);

// Mouse moves camera in 3rd person, steers in 1st person
let mouseX=0;
document.addEventListener('mousemove',e=>{ if(gameOn&&!done&&!paused) mouseX+=e.movementX; });
document.getElementById('c').addEventListener('click',()=>{
  if(viewMode==='1p'&&gameOn&&!done) document.getElementById('c').requestPointerLock();
});
document.addEventListener('pointerlockchange',()=>{
  const locked=document.pointerLockElement===document.getElementById('c');
  if(viewMode==='1p') document.getElementById('esc-hint').textContent=locked?'ESC = PAUSE':'ESC = PAUSE  |  CLICK TO LOCK MOUSE';
});

let tiltX=0;
window.addEventListener('deviceorientation',e=>{if(e.gamma!==null)tiltX=Math.max(-45,Math.min(45,e.gamma));},{passive:true});

// ==============================================
//  GAME STATE
// ==============================================
const carPos=new THREE.Vector3();
let carAngle=0,velAngle=0,steer=0,driftAmt=0,isDrift=false;
let lap=1,lastCP=-1,cpTotal=0,gameOn=false,done=false,paused=false,startTime=0;
let bounce=0,bounceV=0;
let fpsFrames=0,fpsLast=performance.now(),currentFPS=0,fpsEl=null;
let playerName='',playerColor='';
let fwdKey=false,bkKey=false;
let myCarMesh=null;
// 3rd person cam state
let camX=0,camZ=0,camYpos=0,camReady=false;
// 1st person cam state
let fpCamPitch=0,fpCamRoll=0;
const MAX_ST=0.85,ST_RTN=0.10,BASE_TURN=0.044,GRIP=0.87,DGRIP=0.50;
const mmCtx=document.getElementById('mmc').getContext('2d');

function aDiff(a,b){let d=a-b;while(d>Math.PI)d-=Math.PI*2;while(d<-Math.PI)d+=Math.PI*2;return d;}

// ==============================================
//  MENU WIRING
// ==============================================
document.getElementById('btn-go').onclick=()=>launch(false);
document.getElementById('inp-name').addEventListener('keydown',e=>{if(e.key==='Enter')launch(false);});
document.getElementById('btn-mp').onclick=()=>{
  document.getElementById('menu').classList.add('off');
  document.getElementById('mp-modal').classList.remove('off');
  startMPConnect();
};
document.getElementById('btn-cxl').onclick=()=>{
  document.getElementById('mp-modal').classList.add('off');
  document.getElementById('menu').classList.remove('off');
};
document.getElementById('btn-conn').onclick=()=>launch(true);
document.getElementById('btn-resume').onclick=()=>{
  paused=false;
  document.getElementById('pause').classList.add('off');
};

async function startMPConnect(){
  const st=document.getElementById('mp-status');
  const btn=document.getElementById('btn-conn');
  btn.disabled=true; st.className=''; st.textContent='Testing connection ';
  try{
    const res=await fetch(`${FB}/apexdrift/players.json?shallow=true`,{signal:AbortSignal.timeout(7000)});
    if(!res.ok)throw new Error('HTTP '+res.status+'   Check Firebase rules allow read+write for /apexdrift/players');
    isMP=true;
    st.className='ok'; st.textContent='  Connected! Ready to race online.';
    btn.disabled=false;
  }catch(err){
    st.className='err';
    st.textContent=(err.message.includes('401')||err.message.includes('403'))
      ? 'Firebase rules blocking access.\nIn Firebase Console   Realtime Database   Rules\nSet read+write = true for /apexdrift/players'
      : '  '+err.message;
    btn.textContent='RETRY'; btn.disabled=false;
    btn.onclick=()=>{btn.textContent='JOIN RACE';btn.onclick=()=>launch(true);startMPConnect();};
  }
}

// ==============================================
//  LAUNCH
// ==============================================
function launch(mp){
  playerName=document.getElementById('inp-name').value.trim()||'Driver';
  playerColor=pickedColor;
  document.getElementById('menu').classList.add('off');
  document.getElementById('mp-modal').classList.add('off');

  initThree(); buildTrack();

  // Place car at track start
  const sp=trkPt(.005); carPos.set(sp.x,.4,sp.z);
  const st=trkTan(.005); carAngle=Math.atan2(st.x,st.z); velAngle=carAngle;

  // Spawn player car mesh
  myCarMesh=buildCarMesh(playerColor);
  myCarMesh.position.copy(carPos);
  myCarMesh.rotation.y=carAngle;
  const myLabel=makeLabel(playerName,playerColor);
  myLabel.position.set(0,2.5,0); myCarMesh.add(myLabel);
  scene.add(myCarMesh);

  // Show correct cockpit overlay
  document.getElementById('cockpit').style.display=viewMode==='1p'?'block':'none';
  document.getElementById('hud').style.display='block';
  fpsEl=document.getElementById('fps-display');
  document.getElementById('lap-val').textContent=`1 / ${TOTAL_LAPS}`;

  if(mp&&isMP){ setupMP(); document.getElementById('mp-dot').style.display='block'; }

  countdown(()=>{ gameOn=true; startTime=Date.now(); });
  requestAnimationFrame(loop);
}

function countdown(cb){
  const el=document.getElementById('cdv');
  const show=(t,c='')=>{el.className='show '+c;el.textContent=t;setTimeout(()=>el.className='',680);};
  show('3');setTimeout(()=>show('2'),1000);setTimeout(()=>show('1'),2000);setTimeout(()=>{show('GO!','go');cb();},3000);
}

// ==============================================
//  PHYSICS TICK
// ==============================================
function tickDynamic(frameDt){
  if(!gameOn||done||paused)return;
  const DT=frameDt; // real elapsed time this frame

  fwdKey=keys['w']||keys['W'];
  bkKey =keys['s']||keys['S'];
  const kL=keys['a']||keys['A'];
  const kR=keys['d']||keys['D'];
  const hbk=keys[' ']||keys['Shift']||keys['ShiftLeft']||keys['ShiftRight'];

  // Shift clutch
  if(shiftTimer>0){shiftTimer-=DT*1000;clutch=Math.max(0,shiftTimer/SHIFT_MS);}else clutch=0;

  // ARCADE PHYSICS -- always moves instantly when W pressed
  const topMS=GEAR_TOP_MS[gear];
  const acc=GEAR_ACC[gear]*(1-clutch*0.65);
  const REV_MAX = 25; // max reverse speed m/s (~90 km/h)
  if(fwdKey){
    if(carSpdMS < 0) carSpdMS = Math.min(0, carSpdMS + acc*3*DT); // brake from reverse
    else carSpdMS = Math.min(carSpdMS + acc*DT, topMS);
  } else if(bkKey){
    if(carSpdMS > 0) carSpdMS = Math.max(0, carSpdMS - acc*2.5*DT); // brake from forward
    else carSpdMS = Math.max(carSpdMS - acc*0.4*DT, -REV_MAX); // reverse
  } else {
    // Friction — slow to stop
    if(Math.abs(carSpdMS) < 1) carSpdMS = 0;
    else carSpdMS -= Math.sign(carSpdMS) * 5 * DT;
  }
  // Aero drag (only at high positive speed)
  if(carSpdMS > 0) carSpdMS = Math.max(0, carSpdMS - 0.00035*carSpdMS*carSpdMS*DT);
  carSpd = carSpdMS / 277.8; // signed — negative = reversing

  // Visual RPM
  const rpmT=RPM_IDLE+Math.min(1,carSpdMS/Math.max(topMS,1))*(RPM_LIMIT-RPM_IDLE)*(1+clutch*0.4);
  rpm+=(Math.min(RPM_LIMIT,rpmT)-rpm)*0.14;
  rpm=Math.max(RPM_IDLE,Math.min(RPM_LIMIT,rpm));

  // Steering
  if(viewMode==='1p'){
    steer+=mouseX*.002; mouseX=0;
  } else {
    steer+=mouseX*.0008; mouseX=0; // gentler in 3p
  }
  steer+=tiltX*.0005;
  if(kL)steer-=.06; if(kR)steer+=.06;
  if(!kL&&!kR)steer*=(1-ST_RTN);
  steer=Math.max(-MAX_ST,Math.min(MAX_ST,steer));

  // Turn car (less at high speed)
  if(carSpdMS>.3){
    const sf=1/(1+carSpdMS*.016);
    carAngle-=BASE_TURN*(steer/MAX_ST)*sf*(1+driftAmt*.2);
  }

  // Drift
  isDrift=hbk&&carSpdMS>3;
  const grip=isDrift?DGRIP:GRIP;
  velAngle+=aDiff(carAngle,velAngle)*(1-grip);
  driftAmt+=(Math.abs(aDiff(velAngle,carAngle))-driftAmt)*.10;
  if(isDrift)carSpdMS=Math.max(0,carSpdMS-driftAmt*2*DT);

  // Move car
  carPos.x+=Math.sin(velAngle)*carSpd;
  carPos.z+=Math.cos(velAngle)*carSpd;
  carPos.y=.4;

  resolveWalls();
  resolveCarCollisions();

  // Update player car mesh
  if(myCarMesh){
    myCarMesh.position.copy(carPos);
    myCarMesh.rotation.y=carAngle;
    myCarMesh.rotation.z+=((-steer*.07*Math.min(carSpdMS/60,1))-myCarMesh.rotation.z)*.12;
  }

  if((isDrift||driftAmt>.08)&&Math.random()<.5)spawnSmoke(carPos);
  tickSmoke();

  // HUD
  document.getElementById('spd').textContent=Math.round(carSpdMS*3.6);
  // FPS counter
  fpsFrames++; const fpsNow=performance.now();
  if(fpsNow-fpsLast>500){ currentFPS=Math.round(fpsFrames*1000/(fpsNow-fpsLast)); fpsFrames=0; fpsLast=fpsNow; if(fpsEl)fpsEl.textContent=currentFPS+' FPS'; }
  const rpmPct=Math.min(100,rpm/RPM_LIMIT*100);
  const rb=document.getElementById('rpm-bar');
  rb.style.width=rpmPct+'%';
  rb.style.background=rpm>RPM_LIMIT*.88?'linear-gradient(90deg,var(--hot),#f60)':'linear-gradient(90deg,var(--neon),var(--gold))';
  if(viewMode==='1p') document.getElementById('wheel-svg').style.transform=`rotate(${steer*(180/Math.PI)*2.4}deg)`;

  checkCPs(); checkWW();
  if(isMP)syncFB();
}

// ==============================================
//  CAMERA
// ==============================================
function updateCam(){
  if(viewMode==='3p'){
    // Chase cam   behind and above
    const tx=carPos.x-Math.sin(carAngle)*12;
    const tz=carPos.z-Math.cos(carAngle)*12;
    const ty=carPos.y+5;
    if(!camReady){camX=tx;camZ=tz;camYpos=ty;camReady=true;}
    camX+=(tx-camX)*.09; camZ+=(tz-camZ)*.09; camYpos+=(ty-camYpos)*.09;
    bounceV+=(0-bounce)*.2;bounce+=bounceV;
    if(carSpdMS>5)bounceV+=(Math.random()-.5)*(carSpdMS/300)*.003;
    bounceV*=.9;
    camera.position.set(camX,camYpos+bounce,camZ);
    camera.lookAt(carPos.x+Math.sin(carAngle)*3,carPos.y+1,carPos.z+Math.cos(carAngle)*3);
  } else {
    // FPS   inside cockpit
    const ex=carPos.x+Math.sin(carAngle)*.5,ez=carPos.z+Math.cos(carAngle)*.5;
    bounceV+=(0-bounce)*.18;bounce+=bounceV;
    if(carSpdMS>1)bounceV+=(Math.random()-.5)*(carSpdMS/138.9)*.005;
    bounceV*=.88;
    camera.position.set(ex,carPos.y+1.1+bounce,ez);
    const tP=fwdKey?-.04:bkKey?.05:0; fpCamPitch+=(tP-fpCamPitch)*.06;
    const tR=-steer*.04*Math.min(carSpdMS/138.9,1); fpCamRoll+=(tR-fpCamRoll)*.08;
    camera.rotation.order='YXZ';
    camera.rotation.y=carAngle+Math.PI;
    camera.rotation.x=fpCamPitch; camera.rotation.z=fpCamRoll;
  }
}

// ==============================================
//  CHECKPOINTS
// ==============================================
function checkCPs(){
  for(let i=0;i<CPS.length;i++){
    if(carPos.distanceTo(CPS[i].pos)<22&&i!==lastCP&&i===(lastCP+1)%CP_COUNT){
      lastCP=i;cpTotal++;
      if(i===0&&cpTotal>1){
        if(lap<TOTAL_LAPS){lap++;document.getElementById('lap-val').textContent=`${lap} / ${TOTAL_LAPS}`;}
        else finish();
      }
    }
  }
}
let wwT=0;
function checkWW(){
  const tan=trkTan(lastCP<0?0:lastCP/CP_COUNT);
  const dir=new THREE.Vector3(Math.sin(carAngle),0,Math.cos(carAngle));
  const ww=document.getElementById('wrong-way');
  if(dir.dot(tan)<-.5&&carSpdMS>1.5&&carSpdMS>0){wwT++;if(wwT>35)ww.style.display='block';}
  else{wwT=0;ww.style.display='none';}
}
function finish(){
  done=true;
  document.getElementById('fin-sub').textContent=`Time: ${((Date.now()-startTime)/1000).toFixed(2)}s`;
  document.getElementById('finish').classList.add('show');
  if(document.pointerLockElement)document.exitPointerLock();
  if(isMP){fbRemoveMe();stopPolling();}
}

// ==============================================
//  MINIMAP
// ==============================================
function drawMM(){
  const W=120,H=120,sx=W/(TW*2+60),sz=H/(TH*2+60),cx=W/2,cz=H/2;
  mmCtx.clearRect(0,0,W,H);
  mmCtx.beginPath();
  TPTS.forEach((p,i)=>{const mx=cx+p.x*sx,mz=cz+p.z*sz;i===0?mmCtx.moveTo(mx,mz):mmCtx.lineTo(mx,mz);});
  mmCtx.closePath();mmCtx.strokeStyle='rgba(255,255,255,.18)';mmCtx.lineWidth=5;mmCtx.stroke();
  Object.values(remotes).forEach(r=>{
    const mx=cx+r.mesh.position.x*sx,mz=cz+r.mesh.position.z*sz;
    mmCtx.beginPath();mmCtx.arc(mx,mz,4,0,Math.PI*2);mmCtx.fillStyle=r.color;mmCtx.fill();
  });
  const mx=cx+carPos.x*sx,mz=cz+carPos.z*sz;
  mmCtx.beginPath();mmCtx.arc(mx,mz,5,0,Math.PI*2);mmCtx.fillStyle='#00ffe7';mmCtx.fill();
  mmCtx.beginPath();mmCtx.moveTo(mx,mz);mmCtx.lineTo(mx+Math.sin(carAngle)*9,mz+Math.cos(carAngle)*9);
  mmCtx.strokeStyle='#00ffe7';mmCtx.lineWidth=2;mmCtx.stroke();
}

// ==============================================
//  MULTIPLAYER
// ==============================================
function setupMP(){
  myId=Math.random().toString(36).slice(2,10);

  // Method 1: beforeunload (works most of the time)
  window.addEventListener('beforeunload', e=>{ fbRemoveMe(); });

  // Method 2: pagehide (mobile + some desktop)
  window.addEventListener('pagehide', ()=>{ fbRemoveMe(); });

  // Method 3: visibilitychange (tab switch / close)
  document.addEventListener('visibilitychange', ()=>{
    if(document.visibilityState==='hidden'){ fbRemoveMe(); stopPolling(); }
    else if(isMP){ startPolling(); syncFB(); } // rejoin on tab focus
  });

  // Method 4: keepalive fetch with sendBeacon on unload
  window.addEventListener('unload', ()=>{
    try{
      // sendBeacon survives tab close better than fetch
      const url = FB+'/apexdrift/players/'+myId+'.json?x-http-method-override=DELETE';
      navigator.sendBeacon(url, JSON.stringify(null));
    }catch(e){}
  });

  startPolling();
}

function handleRemotes(data){
  if(!scene||!data)return;
  const lb=[{id:myId,name:playerName,color:playerColor,lap,cp:cpTotal}];
  Object.entries(data).forEach(([id,p])=>{
    if(id===myId||!p||typeof p.x==='undefined')return;
    if(!remotes[id]){
      const mesh=buildCarMesh(p.color||'#ff3a5c');
      mesh.position.set(p.x,.4,p.z);
      mesh.rotation.y=p.angle||0;
      const lbl=makeLabel(p.name||'Driver',p.color||'#ff3a5c');
      lbl.position.set(0,2.5,0);mesh.add(lbl);
      scene.add(mesh);
      remotes[id]={mesh,color:p.color||'#ff3a5c',lap:1,cp:0,tx:p.x,tz:p.z,ta:p.angle||0,vx:0,vz:0,lastUp:Date.now()};
    } else {
      const r=remotes[id];
      const dt=Math.max(.01,(Date.now()-r.lastUp)/1000);
      // Use server timestamp if available for more accurate velocity
      const serverDt=p.t ? Math.max(0.01,(p.t-r.serverT||Date.now()-30)/1000) : dt;
      r.vx=(p.x-r.tx)/serverDt; r.vz=(p.z-r.tz)/serverDt;
      r.tx=p.x; r.tz=p.z; r.ta=p.angle||0;
      r.lap=p.lap||1; r.cp=p.cp||0;
      r.lastUp=Date.now(); r.serverT=p.t||Date.now();
    }
    lb.push({id,name:p.name||'Driver',color:p.color||'#888',lap:p.lap||1,cp:p.cp||0});
  });
  Object.keys(remotes).forEach(id=>{ if(!data[id]){scene.remove(remotes[id].mesh);delete remotes[id];} });
  renderLB(lb);
}

function updateRemotes(){
  if(!isMP)return;
  const now=Date.now();
  Object.entries(remotes).forEach(([id,r])=>{
    if(now-r.lastUp>3000){scene.remove(r.mesh);delete remotes[id];return;}
    // Dead reckoning: predict EXACT position right now using velocity + time elapsed
    const elapsed=(now-r.lastUp)/1000; // seconds since last real update
    const predX=r.tx + r.vx*elapsed;
    const predZ=r.tz + r.vz*elapsed;
    // Smooth lerp toward predicted position — runs every frame at 60fps
    // This makes it look like 60fps updates even though network is 30Hz
    r.mesh.position.x+=(predX-r.mesh.position.x)*0.5;
    r.mesh.position.z+=(predZ-r.mesh.position.z)*0.5;
    r.mesh.position.y=0.4;
    // Smooth rotation
    r.mesh.rotation.y+=aDiff(r.ta,r.mesh.rotation.y)*0.4;
  });
}

function syncFB(){
  const now=Date.now();if(now-lastSync<30)return;lastSync=now;
  fbWrite({x:+carPos.x.toFixed(2),z:+carPos.z.toFixed(2),y:.4,angle:+carAngle.toFixed(3),lap,cp:cpTotal,name:playerName,color:playerColor,gear:gear+1,kmh:Math.round(carSpdMS*3.6),t:now});
}

function renderLB(players){
  players.sort((a,b)=>b.lap-a.lap||b.cp-a.cp);
  const list=document.getElementById('lb-list');list.innerHTML='';
  players.slice(0,6).forEach((p,i)=>{
    const d=document.createElement('div');d.className='lbe';
    d.innerHTML=`<span class="lbe-pos">${i+1}</span><span class="lbe-dot" style="background:${p.color}"></span><span class="lbe-name">${p.name}</span><span class="lbe-lap">L${p.lap}</span>`;
    list.appendChild(d);
  });
}

// ==============================================
//  UNLIMITED FPS LOOP — framerate independent physics
// ==============================================
let lastFrameTime = performance.now();
const MAX_DT = 1/30; // clamp to avoid spiral of death if tab was hidden

function loop(){
  requestAnimationFrame(loop);
  const now = performance.now();
  const frameDt = Math.min((now - lastFrameTime) / 1000, MAX_DT);
  lastFrameTime = now;

  if(!paused){
    tickDynamic(frameDt);
    updateCam();
    updateRemotes();
    // Only redraw minimap every 4 frames (not needed at 200fps)
    if(Math.random()<0.25) drawMM();
  }
  renderer.render(scene,camera);
}
</script>
</body>
</html>
