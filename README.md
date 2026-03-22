<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>APEX DRIFT</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Barlow+Condensed:wght@400;600;700&display=swap');
:root{--neon:#00ffe7;--hot:#ff3a5c;--gold:#ffc820;--bg:#060810;--bdr:rgba(0,255,231,.15)}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{width:100%;height:100%;overflow:hidden;background:var(--bg);color:#e8eeff;font-family:'Barlow Condensed',sans-serif}
canvas#c{display:block;position:fixed;inset:0;width:100%;height:100%;z-index:1}
/* all UI above canvas */
.screen{position:fixed;inset:0;z-index:100;display:flex;align-items:center;justify-content:center}
.off{display:none!important}

/* ROTATE */
#rot{background:#060810;flex-direction:column;align-items:center;justify-content:center;gap:16px;text-align:center;padding:40px}
.r-icon{font-size:60px;animation:rspin 2s ease-in-out infinite}
@keyframes rspin{0%,100%{transform:rotate(0deg)}50%{transform:rotate(90deg)}}
#rot h2{font-family:'Bebas Neue',sans-serif;font-size:clamp(28px,8vw,44px);letter-spacing:5px;color:var(--neon)}
#rot p{font-size:14px;letter-spacing:2px;color:rgba(255,255,255,.4)}

/* DEVICE PICKER */
#dp{background:radial-gradient(ellipse 120% 80% at 50% 50%,#0c1e3a 0%,#060810 100%);flex-direction:column}
.dp-wrap{text-align:center;padding:20px;width:100%;max-width:660px;margin:0 auto}
.dp-logo{font-family:'Bebas Neue',sans-serif;font-size:clamp(54px,13vw,98px);letter-spacing:6px;line-height:.88;background:linear-gradient(155deg,#fff 25%,var(--neon));-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:4px}
.dp-sub{font-size:clamp(9px,2vw,12px);letter-spacing:7px;color:var(--neon);opacity:.4;text-transform:uppercase;margin-bottom:clamp(16px,4vh,38px)}
/* detected label - above grid */
#dp-detected{min-height:22px;font-size:clamp(11px,2.2vw,14px);letter-spacing:3px;color:var(--neon);margin-bottom:14px;opacity:.9}
.dp-grid{display:flex;gap:clamp(8px,2.5vw,18px);justify-content:center;margin-bottom:clamp(16px,3.5vh,28px)}
.dp-btn{
  width:clamp(100px,27vw,182px);padding:clamp(14px,3vw,26px) clamp(10px,2vw,18px);
  background:rgba(0,255,231,.04);border:1.5px solid rgba(0,255,231,.18);
  color:#fff;cursor:pointer;border-radius:6px;transition:all .2s;
  display:flex;flex-direction:column;align-items:center;gap:6px;
}
.dp-btn:hover,.dp-btn:active{background:rgba(0,255,231,.12);border-color:var(--neon);transform:translateY(-2px);box-shadow:0 6px 24px rgba(0,255,231,.18)}
.dp-btn.detected{border-color:var(--neon);box-shadow:0 0 22px rgba(0,255,231,.28)}
.dp-icon{font-size:clamp(30px,7vw,48px);line-height:1}
.dp-name{font-family:'Bebas Neue',sans-serif;font-size:clamp(17px,4.5vw,27px);letter-spacing:4px;color:var(--neon)}
.dp-hint{font-size:clamp(7px,1.6vw,10px);letter-spacing:2px;color:rgba(255,255,255,.3);text-transform:uppercase}
#btn-autodetect{
  padding:10px 22px;background:transparent;border:1px solid rgba(0,255,231,.3);
  color:var(--neon);font-family:'Bebas Neue',sans-serif;font-size:clamp(13px,3vw,16px);
  letter-spacing:4px;cursor:pointer;border-radius:4px;transition:all .18s;
}
#btn-autodetect:hover,#btn-autodetect:active{background:rgba(0,255,231,.12);border-color:var(--neon)}

/* MENU */
#menu{background:radial-gradient(ellipse 80% 60% at 50% 55%,#0b1628 0%,#060810 100%)}
.card{width:min(450px,94vw);border:1px solid var(--bdr);background:#080c18;padding:38px 36px;box-shadow:0 0 80px rgba(0,255,231,.05)}
.logo{font-family:'Bebas Neue',sans-serif;font-size:68px;letter-spacing:6px;line-height:.9;background:linear-gradient(160deg,#fff 20%,var(--neon));-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-bottom:3px}
.logo-sub{font-size:10px;letter-spacing:7px;color:var(--neon);opacity:.38;text-transform:uppercase;margin-bottom:26px}
.fl{font-size:10px;letter-spacing:5px;color:var(--neon);opacity:.38;text-transform:uppercase;margin-bottom:5px}
input[type=text]{width:100%;padding:10px 13px;background:rgba(0,0,0,.5);border:1px solid var(--bdr);color:#fff;font-family:'Barlow Condensed',sans-serif;font-size:16px;font-weight:600;outline:none;transition:border-color .2s;margin-bottom:14px}
input[type=text]:focus{border-color:var(--neon)}
input[type=text]::placeholder{color:rgba(255,255,255,.18)}
.swatches{display:flex;gap:7px;margin-bottom:14px;flex-wrap:wrap}
.sw{width:28px;height:28px;border-radius:50%;cursor:pointer;border:3px solid transparent;transition:all .14s}
.sw.on,.sw:hover{border-color:#fff;transform:scale(1.18)}
.vtog{display:flex;margin-bottom:16px;border:1px solid var(--bdr);border-radius:3px;overflow:hidden}
.vbtn{flex:1;padding:9px;background:transparent;color:rgba(255,255,255,.38);font-family:'Bebas Neue',sans-serif;font-size:14px;letter-spacing:3px;border:none;cursor:pointer;transition:all .15s}
.vbtn.on{background:var(--neon);color:#000}
.vbtn:not(.on):hover{background:rgba(0,255,231,.1);color:var(--neon)}
.btn-go{width:100%;padding:13px;background:var(--neon);color:#000;font-family:'Bebas Neue',sans-serif;font-size:20px;letter-spacing:6px;border:none;cursor:pointer;transition:all .18s;margin-bottom:8px}
.btn-go:hover{background:#fff}.btn-go:active{transform:scale(.97)}
.btn-mp{width:100%;padding:10px;background:transparent;color:rgba(255,255,255,.38);font-family:'Bebas Neue',sans-serif;font-size:14px;letter-spacing:4px;border:1px solid rgba(0,255,231,.2);cursor:pointer;transition:all .18s;display:flex;align-items:center;justify-content:center;gap:8px}
.btn-mp:hover{border-color:var(--neon);color:var(--neon)}
.badge{font-size:9px;background:var(--hot);color:#fff;padding:2px 5px;border-radius:2px;font-family:'Barlow Condensed',sans-serif;font-weight:700;letter-spacing:2px}
.hint{margin-top:10px;font-size:9px;color:rgba(255,255,255,.14);letter-spacing:2px;text-align:center}

/* MP MODAL */
#mpm{background:rgba(0,0,0,.9);backdrop-filter:blur(14px)}
.mbox{width:min(390px,92vw);border:1px solid var(--bdr);background:#090d1c;padding:36px 32px;text-align:center}
.mbox h2{font-family:'Bebas Neue',sans-serif;font-size:26px;letter-spacing:4px;color:var(--neon);margin-bottom:10px}
#mpst{font-size:12px;letter-spacing:1px;margin-bottom:18px;min-height:16px;color:rgba(255,255,255,.4);line-height:1.7;white-space:pre-line}
#mpst.ok{color:var(--neon)}#mpst.err{color:var(--hot)}
.mrow{display:flex;gap:8px}
.mrow button{flex:1;padding:11px;font-family:'Bebas Neue',sans-serif;font-size:15px;letter-spacing:4px;border:none;cursor:pointer;transition:all .15s}
#btn-conn{background:var(--neon);color:#000}#btn-conn:hover{background:#fff}#btn-conn:disabled{opacity:.3;cursor:not-allowed}
#btn-cxl{background:transparent;border:1px solid rgba(255,255,255,.15);color:rgba(255,255,255,.35)}
#btn-cxl:hover{border-color:var(--hot);color:var(--hot)}

/* PAUSE */
#paus{background:rgba(0,0,0,.78);backdrop-filter:blur(10px);flex-direction:column;gap:12px}
#paus h2{font-family:'Bebas Neue',sans-serif;font-size:64px;letter-spacing:6px}
#paus button{padding:11px 42px;font-family:'Bebas Neue',sans-serif;font-size:17px;letter-spacing:5px;border:none;cursor:pointer;transition:all .15s;border-radius:3px}
#btn-res{background:var(--neon);color:#000}#btn-res:hover{background:#fff}
#btn-quit{background:transparent;border:1px solid rgba(255,255,255,.2);color:rgba(255,255,255,.4)}
#btn-quit:hover{border-color:var(--hot);color:var(--hot)}

/* HUD */
#hud{position:fixed;inset:0;z-index:50;pointer-events:none;display:none}
#lap-hud{position:absolute;top:14px;left:50%;transform:translateX(-50%);text-align:center}
#lap-lbl{font-size:9px;letter-spacing:7px;color:rgba(255,255,255,.26);text-transform:uppercase}
#lap-val{font-family:'Bebas Neue',sans-serif;font-size:36px;color:#fff;line-height:1}
#lb{position:absolute;top:14px;left:18px;min-width:140px}
#lb-ttl{font-size:9px;letter-spacing:5px;color:var(--neon);opacity:.3;margin-bottom:6px}
.lbe{display:flex;align-items:center;gap:6px;padding:3px 0;border-bottom:1px solid rgba(255,255,255,.05);font-size:11px;font-weight:600}
.lp{width:13px;color:rgba(255,255,255,.2);font-size:10px}.ld{width:7px;height:7px;border-radius:50%;flex-shrink:0}.ln{flex:1}.ll{color:rgba(255,255,255,.28);font-size:10px}
#gear-hud{position:absolute;top:14px;right:18px;text-align:right}
#gear-lbl{font-size:9px;letter-spacing:5px;color:rgba(255,255,255,.26);text-transform:uppercase}
#gear-val{font-family:'Bebas Neue',sans-serif;font-size:48px;color:var(--gold);line-height:1}
#mm{position:absolute;bottom:145px;left:18px;width:112px;height:112px;border-radius:50%;border:1px solid var(--bdr);overflow:hidden;background:rgba(0,0,0,.5)}
#mmc{width:100%;height:100%}
#spdo{position:absolute;bottom:18px;right:18px;text-align:right}
#spd{font-family:'Bebas Neue',sans-serif;font-size:56px;line-height:1;color:var(--neon)}
#spdU{font-size:8px;letter-spacing:5px;color:rgba(255,255,255,.22)}
#rpmw{position:absolute;bottom:86px;right:18px;width:190px;height:4px;background:rgba(255,255,255,.07);border-radius:2px;overflow:hidden}
#rpmb{height:100%;width:0%;border-radius:2px}
#fps{position:absolute;top:14px;left:50%;margin-left:70px;font-family:'Bebas Neue',sans-serif;font-size:15px;letter-spacing:3px;color:rgba(0,255,231,.28)}
#gf{position:fixed;top:32%;left:50%;transform:translate(-50%,-50%);font-family:'Bebas Neue',sans-serif;font-size:62px;letter-spacing:4px;opacity:0;pointer-events:none;z-index:200;transition:opacity .06s}
#ww{position:fixed;top:40%;left:50%;transform:translate(-50%,-50%);font-family:'Bebas Neue',sans-serif;font-size:58px;color:var(--hot);letter-spacing:8px;text-shadow:0 0 40px rgba(255,58,92,.7);display:none;z-index:200;animation:bl .45s infinite alternate}
@keyframes bl{to{opacity:.2}}
#cdo{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;z-index:300;pointer-events:none}
#cd{font-family:'Bebas Neue',sans-serif;font-size:180px;color:var(--neon);text-shadow:0 0 80px rgba(0,255,231,.55);opacity:0;transform:scale(1.5);transition:opacity .22s,transform .22s}
#cd.show{opacity:1;transform:scale(1)}#cd.go{color:var(--gold)}
#mpdot{position:fixed;top:9px;left:50%;transform:translateX(-50%);font-size:9px;letter-spacing:3px;color:rgba(255,255,255,.18);z-index:200;display:none;pointer-events:none}
#mpdot span{display:inline-block;width:5px;height:5px;border-radius:50%;background:var(--neon);box-shadow:0 0 6px var(--neon);margin-right:4px;vertical-align:middle;animation:pu 1.4s infinite}
@keyframes pu{0%,100%{opacity:1}50%{opacity:.2}}
#vig{position:fixed;inset:0;pointer-events:none;z-index:5;background:radial-gradient(ellipse at center,transparent 50%,rgba(0,0,0,.62) 100%)}
#hf{position:fixed;inset:0;pointer-events:none;z-index:20;opacity:0;background:rgba(255,58,92,.22);transition:opacity .05s}
#fin{background:rgba(0,0,0,.85);backdrop-filter:blur(10px);flex-direction:column;display:none}
#fin.show{display:flex}
#fin h1{font-family:'Bebas Neue',sans-serif;font-size:86px;color:var(--gold);text-shadow:0 0 60px rgba(255,200,32,.6);letter-spacing:8px}
#fin p{font-size:16px;color:rgba(255,255,255,.4);margin-top:5px}
#btn-again{margin-top:28px;padding:12px 44px;background:transparent;border:2px solid var(--neon);color:var(--neon);font-family:'Bebas Neue',sans-serif;font-size:19px;letter-spacing:5px;cursor:pointer;transition:all .15s;pointer-events:all}
#btn-again:hover{background:var(--neon);color:#000}

/* TOUCH */
#tui{position:fixed;inset:0;z-index:60;pointer-events:none;display:none}
.sw-wheel-wrap{
  position:absolute;
  width:clamp(140px,28vw,200px);
  height:clamp(140px,28vw,200px);
  bottom:clamp(80px,15vh,140px);
  left:clamp(10px,2.5vw,26px);
  pointer-events:all;
  touch-action:none;
  filter:drop-shadow(0 0 22px rgba(0,255,231,.3)) drop-shadow(0 0 8px rgba(0,0,0,.9));
}
#jl{transform-origin:center center}
.jk{display:none}
#tb{position:absolute;bottom:clamp(88px,17vh,155px);right:clamp(14px,3.5vw,30px);display:flex;flex-direction:column;gap:7px;align-items:flex-end;pointer-events:all}
.tb{background:rgba(255,255,255,.07);border:2px solid rgba(255,255,255,.2);color:#fff;font-family:'Bebas Neue',sans-serif;letter-spacing:3px;border-radius:8px;cursor:pointer;pointer-events:all;user-select:none;touch-action:manipulation}
.tb:active{background:rgba(0,255,231,.22);border-color:var(--neon)}
#tgas{font-size:clamp(14px,3.5vw,19px);padding:clamp(8px,2vw,13px) clamp(14px,3.5vw,24px);background:rgba(0,255,231,.1);border-color:var(--neon);color:var(--neon)}
#tbrk{font-size:clamp(14px,3.5vw,19px);padding:clamp(7px,1.8vw,12px) clamp(12px,3vw,22px);background:rgba(255,58,92,.1);border-color:var(--hot);color:var(--hot)}
#tdft{font-size:clamp(9px,2.2vw,12px);padding:clamp(6px,1.5vw,9px) clamp(10px,2.5vw,15px);color:var(--gold);border-color:var(--gold)}
.tsh{font-size:clamp(10px,2.4vw,13px);padding:clamp(6px,1.5vw,9px) clamp(11px,2.8vw,17px)}

/* iPhone landscape menu */
@media(min-width:600px) and (max-width:980px) and (orientation:landscape){
  .card{width:min(700px,96vw);padding:14px 22px;display:grid;grid-template-columns:1fr 1fr;gap:0 20px;align-items:start}
  .logo{font-size:clamp(32px,6vw,46px);margin-bottom:2px}
  .logo-sub{font-size:8px;margin-bottom:8px}
  input[type=text]{font-size:13px;padding:7px 10px;margin-bottom:8px}
  .swatches{gap:5px;margin-bottom:8px}.sw{width:20px;height:20px}
  .vtog{margin-bottom:8px}.vbtn{font-size:11px;padding:7px}
  .btn-go{font-size:15px;padding:9px;margin-bottom:5px}.btn-mp{font-size:12px;padding:7px}.hint{font-size:8px}
  #lap-val{font-size:26px}#gear-val{font-size:32px}#spd{font-size:38px}
  #mm{width:78px;height:78px;bottom:112px;left:10px}#spdo{bottom:10px;right:10px}
  #rpmw{width:140px;bottom:80px;right:10px}
}
/* iPad */
@media(min-width:768px) and (max-width:1366px) and (min-height:500px){
  .card{width:min(560px,84vw);padding:40px 44px}
  .logo{font-size:82px}.sw{width:32px;height:32px}
  .btn-go{font-size:22px;padding:15px}.btn-mp{font-size:16px;padding:10px}
  #spd{font-size:62px}#lap-val{font-size:44px}#gear-val{font-size:52px}
  #mm{width:132px;height:132px;bottom:190px}
  .sw-wheel-wrap{width:200px;height:200px;bottom:100px;left:20px}
  #tgas{font-size:22px;padding:14px 32px}#tbrk{font-size:22px;padding:12px 28px}
}



/* KICK / TIMEOUT SCREEN */
#kick-screen{ background:rgba(0,0,0,.92);backdrop-filter:blur(14px);flex-direction:column }
.kick-box{ text-align:center;padding:40px;max-width:500px }
#kick-title{ font-family:'Bebas Neue',sans-serif;font-size:clamp(72px,14vw,110px);letter-spacing:8px;color:var(--hot);text-shadow:0 0 60px rgba(255,58,92,.7);line-height:1;margin-bottom:16px }
#kick-reason{ font-size:clamp(13px,2.5vw,17px);letter-spacing:2px;color:rgba(255,255,255,.6);margin-bottom:24px;line-height:1.8;white-space:pre-line }
#kick-timer{ font-family:'Bebas Neue',sans-serif;font-size:clamp(16px,3vw,22px);letter-spacing:4px;color:rgba(255,255,255,.35);margin-bottom:12px }
#kick-progress-wrap{ width:min(300px,80vw);height:4px;background:rgba(255,255,255,.1);border-radius:2px;overflow:hidden;margin:0 auto }
#kick-progress{ height:100%;width:100%;background:var(--hot);border-radius:2px;transition:width 1s linear }

/* SLIDERS */
.sliders{display:flex;flex-direction:column;gap:10px;margin-bottom:16px}
.sl-row{display:flex;align-items:center;gap:10px}
.sl-label{font-size:9px;letter-spacing:4px;color:var(--neon);opacity:.4;text-transform:uppercase;width:58px;flex-shrink:0}
.sl-track{display:flex;background:rgba(0,0,0,.4);border:1px solid var(--bdr);border-radius:3px;overflow:hidden;flex:1}
.sl-opt{flex:1;padding:8px 4px;text-align:center;font-family:'Bebas Neue',sans-serif;font-size:13px;letter-spacing:3px;color:rgba(255,255,255,.35);cursor:pointer;transition:all .15s}
.sl-opt.sl-on{background:var(--neon);color:#000}
.sl-opt:not(.sl-on):hover{background:rgba(0,255,231,.1);color:var(--neon)}
/* same-size race buttons */
.race-btns{display:flex;gap:8px;margin-bottom:8px}
.race-btn{
  flex:1;padding:13px 8px;
  background:var(--neon);color:#000;
  font-family:'Bebas Neue',sans-serif;font-size:clamp(14px,3.5vw,19px);letter-spacing:4px;
  border:none;cursor:pointer;transition:all .18s;
  display:flex;align-items:center;justify-content:center;gap:6px;
}
.race-btn:hover{background:#fff}.race-btn:active{transform:scale(.97)}
.race-btn-mp{background:rgba(0,255,231,.12);color:var(--neon);border:1px solid var(--neon)}
.race-btn-mp:hover{background:var(--neon);color:#000}

/* ========================
   LIVE CHAT
   ======================== */
#chat-box{
  position:fixed;bottom:160px;right:18px;
  width:clamp(220px,28vw,300px);
  z-index:60;pointer-events:all;
  display:none;
  flex-direction:column;
  gap:0;
}
#chat-msgs{
  background:rgba(6,8,16,.82);
  border:1px solid rgba(0,255,231,.15);
  border-bottom:none;
  height:clamp(120px,18vh,180px);
  overflow-y:auto;
  padding:8px 10px;
  display:flex;flex-direction:column;gap:3px;
  border-radius:6px 6px 0 0;
}
#chat-msgs::-webkit-scrollbar{width:3px}
#chat-msgs::-webkit-scrollbar-thumb{background:rgba(0,255,231,.3)}
.chat-msg{font-size:clamp(10px,1.8vw,12px);letter-spacing:.5px;line-height:1.5;word-break:break-word}
.chat-msg .cm-name{font-weight:700;margin-right:5px}
.chat-msg.cm-sys{color:rgba(0,255,231,.5);font-style:italic;font-size:10px}
.chat-msg.cm-mine .cm-name{color:var(--neon)}
.chat-msg:not(.cm-mine):not(.cm-sys) .cm-name{color:var(--gold)}
#chat-input-row{
  display:flex;gap:0;
}
#chat-in{
  flex:1;padding:7px 10px;
  background:rgba(6,8,16,.9);
  border:1px solid rgba(0,255,231,.2);
  border-right:none;
  color:#fff;font-family:'Barlow Condensed',sans-serif;font-size:13px;
  outline:none;border-radius:0 0 0 6px;
}
#chat-in:focus{border-color:rgba(0,255,231,.5)}
#chat-in::placeholder{color:rgba(255,255,255,.22)}
#chat-send{
  padding:7px 12px;background:var(--neon);color:#000;
  font-family:'Bebas Neue',sans-serif;font-size:13px;letter-spacing:2px;
  border:none;cursor:pointer;border-radius:0 0 6px 0;
  transition:background .15s;
}
#chat-send:hover{background:#fff}
/* unread badge */
#chat-toggle{
  position:fixed;bottom:118px;right:18px;
  width:36px;height:36px;border-radius:50%;
  background:rgba(0,255,231,.12);border:1px solid rgba(0,255,231,.3);
  color:var(--neon);font-size:18px;cursor:pointer;
  display:none;align-items:center;justify-content:center;
  z-index:61;transition:all .15s;
}
#chat-toggle:hover{background:rgba(0,255,231,.22)}
#chat-badge{
  position:absolute;top:-4px;right:-4px;
  width:16px;height:16px;border-radius:50%;
  background:var(--hot);color:#fff;
  font-size:9px;font-family:'Barlow Condensed',sans-serif;font-weight:700;
  display:none;align-items:center;justify-content:center;
}

</style>
</head>
<body>
<canvas id="c"></canvas>
<div id="vig"></div>
<div id="hf"></div>
<div id="gf"></div>

<!-- ROTATE (iPhone portrait only) -->
<div class="screen off" id="rot">
  <div class="r-icon">&#x1F4F1;</div>
  <h2>ROTATE YOUR PHONE</h2>
  <p>APEX DRIFT plays in landscape mode</p>
</div>

<!-- DEVICE PICKER -->
<div class="screen" id="dp">
  <div class="dp-wrap">
    <div class="dp-logo">APEX<br>DRIFT</div>
    <div class="dp-sub">Select Your Device</div>
    <div id="dp-detected"></div>
    <div class="dp-grid">
      <button class="dp-btn" id="dpL" onclick="setDev('laptop')">
        <div class="dp-icon">&#x1F4BB;</div>
        <div class="dp-name">LAPTOP</div>
        <div class="dp-hint">Keyboard + Mouse</div>
      </button>
      <button class="dp-btn" id="dpI" onclick="setDev('ipad')">
        <div class="dp-icon">&#x1F4F1;</div>
        <div class="dp-name">iPAD</div>
        <div class="dp-hint">Touch Controls</div>
      </button>
      <button class="dp-btn" id="dpP" onclick="setDev('phone')">
        <div class="dp-icon">&#x1F4DE;</div>
        <div class="dp-name">iPHONE</div>
        <div class="dp-hint">Landscape + Touch</div>
      </button>
    </div>
    <button id="btn-autodetect" onclick="doAutoDetect()">&#x1F50D; AUTO DETECT MY DEVICE</button>
  </div>
</div>

<!-- MENU -->
<div class="screen off" id="menu">
  <div class="card">
    <div class="logo">APEX<br>DRIFT</div>
    <div class="logo-sub">Multiplayer Racing</div>
    <div class="fl">Driver Name</div>
    <input type="text" id="iname" placeholder="Enter callsign..." maxlength="16"/>
    <div class="fl">Car Color</div>
    <div class="swatches" id="swatch"></div>
    <div class="sliders">
      <div class="sl-row">
        <div class="sl-label">CAMERA</div>
        <div class="sl-track" id="sl-cam">
          <div class="sl-opt sl-on" id="sl-cam-3p" onclick="setSl('cam','3p')">3RD</div>
          <div class="sl-opt" id="sl-cam-1p" onclick="setSl('cam','1p')">1ST</div>
        </div>
      </div>
      <div class="sl-row">
        <div class="sl-label">GEARBOX</div>
        <div class="sl-track" id="sl-trns">
          <div class="sl-opt sl-on" id="sl-trns-auto" onclick="setSl('trns','auto')">AUTO</div>
          <div class="sl-opt" id="sl-trns-manual" onclick="setSl('trns','manual')">MANUAL</div>
        </div>
      </div>
      <div class="sl-row">
        <div class="sl-label">ENGINE</div>
        <div class="sl-track" id="sl-eng">
          <div class="sl-opt sl-on" id="sl-eng-petrol" onclick="setSl('eng','petrol')">&#x26FD; PETROL</div>
          <div class="sl-opt" id="sl-eng-electric" onclick="setSl('eng','electric')">&#x26A1; ELECTRIC</div>
        </div>
      </div>
    </div>
    <div class="race-btns">
      <button class="race-btn" id="btn-solo">&#x25B6; SOLO</button>
      <button class="race-btn race-btn-mp" id="btn-mp">&#x26A1; ONLINE <span class="badge">MP</span></button>
    </div>
    <div class="hint" id="ctrl-hint">W=Gas S=Brake A/D=Steer Space=Drift ESC=Pause</div>
  </div>
</div>

<!-- MP MODAL -->
<div class="screen off" id="mpm">
  <div class="mbox">
    <h2>Multiplayer</h2>
    <div id="mpst">Connecting to server...</div>
    <div class="mrow">
      <button id="btn-conn" disabled>JOIN RACE</button>
      <button id="btn-cxl">CANCEL</button>
    </div>
  </div>
</div>

<!-- PAUSE -->
<div class="screen off" id="paus">
  <h2>PAUSED</h2>
  <button id="btn-res">RESUME</button>
  <button id="btn-quit">QUIT TO MENU</button>
</div>

<!-- HUD -->
<div id="hud">
  <div id="lb"><div id="lb-ttl">Standings</div><div id="lb-list"></div></div>
  <div id="lap-hud"><div id="lap-lbl">Lap</div><div id="lap-val">1/3</div></div>
  <div id="gear-hud"><div id="gear-lbl">Gear</div><div id="gear-val">1</div></div>
  <div id="mm"><canvas id="mmc" width="112" height="112"></canvas></div>
  <div id="spdo"><div id="spd">0</div><div id="spdU">KM/H</div></div>
  <div id="rpmw"><div id="rpmb"></div></div>
  <div id="fps">-- FPS</div>
</div>
<div id="mpdot"><span></span>ONLINE</div>
<div id="ww">WRONG WAY</div>
<div id="cdo"><div id="cd"></div></div>

<!-- TOUCH UI -->
<div id="tui">
  <!-- Steering wheel touch control -->
  <div id="jl" class="sw-wheel-wrap">
    <svg id="sw-svg" viewBox="0 0 240 240" xmlns="http://www.w3.org/2000/svg" style="width:100%;height:100%;display:block">
      <defs>
        <linearGradient id="rG" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" stop-color="#2a2e45"/>
          <stop offset="50%" stop-color="#181c2c"/>
          <stop offset="100%" stop-color="#0e1018"/>
        </linearGradient>
        <linearGradient id="sG" x1="0%" y1="0%" x2="0%" y2="100%">
          <stop offset="0%" stop-color="#22273a"/>
          <stop offset="100%" stop-color="#0d1018"/>
        </linearGradient>
        <linearGradient id="hG" x1="30%" y1="20%" x2="70%" y2="80%">
          <stop offset="0%" stop-color="#1a2233"/>
          <stop offset="100%" stop-color="#080b12"/>
        </linearGradient>
        <linearGradient id="bG" x1="30%" y1="20%" x2="70%" y2="80%">
          <stop offset="0%" stop-color="#00ffe7"/>
          <stop offset="100%" stop-color="#008a7c"/>
        </linearGradient>
      </defs>
      <circle cx="120" cy="120" r="112" fill="none" stroke="#00ffe7" stroke-width="1" opacity="0.1"/>
      <circle cx="120" cy="120" r="100" fill="none" stroke="url(#rG)" stroke-width="28"/>
      <circle cx="120" cy="120" r="100" fill="none" stroke="#fff" stroke-width="1" stroke-dasharray="1.5 11" opacity="0.04"/>
      <circle cx="120" cy="120" r="86" fill="none" stroke="#fff" stroke-width="0.8" opacity="0.05"/>
      <path d="M 52 50 A 100 100 0 0 1 188 50" fill="none" stroke="#00ffe7" stroke-width="10" stroke-linecap="round" opacity="0.92"/>
      <path d="M 28 148 A 100 100 0 0 1 75 210" fill="none" stroke="#ff3a5c" stroke-width="10" stroke-linecap="round" opacity="0.88"/>
      <path d="M 165 210 A 100 100 0 0 1 212 148" fill="none" stroke="#ff3a5c" stroke-width="10" stroke-linecap="round" opacity="0.88"/>
      <rect x="113" y="26" width="14" height="72" rx="5" fill="#0d1018"/>
      <rect x="115" y="28" width="10" height="68" rx="3" fill="url(#sG)"/>
      <rect x="119" y="28" width="2" height="68" rx="1" fill="#00ffe7" opacity="0.2"/>
      <line x1="120" y1="120" x2="44" y2="206" stroke="#0d1018" stroke-width="16" stroke-linecap="round"/>
      <line x1="120" y1="120" x2="44" y2="206" stroke="url(#sG)" stroke-width="11" stroke-linecap="round"/>
      <line x1="120" y1="120" x2="44" y2="206" stroke="#00ffe7" stroke-width="1.5" stroke-linecap="round" opacity="0.14"/>
      <line x1="120" y1="120" x2="196" y2="206" stroke="#0d1018" stroke-width="16" stroke-linecap="round"/>
      <line x1="120" y1="120" x2="196" y2="206" stroke="url(#sG)" stroke-width="11" stroke-linecap="round"/>
      <line x1="120" y1="120" x2="196" y2="206" stroke="#00ffe7" stroke-width="1.5" stroke-linecap="round" opacity="0.14"/>
      <circle cx="120" cy="120" r="38" fill="url(#hG)"/>
      <circle cx="120" cy="120" r="38" fill="none" stroke="#2a3050" stroke-width="1.5"/>
      <polygon points="120,86 146,93 154,120 146,147 120,154 94,147 86,120 94,93" fill="#0c1020" stroke="#1e2535" stroke-width="1"/>
      <circle cx="120" cy="120" r="20" fill="#0a0d18" stroke="#00ffe7" stroke-width="1.2" opacity="0.7"/>
      <circle cx="120" cy="120" r="14" fill="url(#bG)" opacity="0.85"/>
      <ellipse cx="115" cy="114" rx="5" ry="3.5" fill="white" opacity="0.22" transform="rotate(-25 115 114)"/>
      <rect x="117" y="11" width="6" height="12" rx="3" fill="#00ffe7"/>
      <rect x="226" y="117" width="10" height="6" rx="2" fill="#333" opacity="0.55"/>
      <rect x="4"   y="117" width="10" height="6" rx="2" fill="#333" opacity="0.55"/>
      <rect x="117" y="226" width="6" height="10" rx="2" fill="#333" opacity="0.38"/>
      <text x="120" y="117" text-anchor="middle" fill="#00443e" font-size="7" font-family="monospace" letter-spacing="1.5" font-weight="bold">APEX</text>
      <text x="120" y="128" text-anchor="middle" fill="#003028" font-size="5.5" font-family="monospace" letter-spacing="2">DRIFT</text>
    </svg>
  </div>
  <div id="tb">
    <button class="tb" id="tgas">GAS</button>
    <button class="tb" id="tbrk">BRAKE</button>
    <button class="tb" id="tdft">DRIFT</button>
    <button class="tb tsh" id="tup">GEAR UP</button>
    <button class="tb tsh" id="tdn">GEAR DOWN</button>
  </div>
</div>


<!-- LIVE CHAT -->
<button id="chat-toggle" title="Chat">
  &#x1F4AC;
  <span id="chat-badge"></span>
</button>
<div id="chat-box">
  <div id="chat-msgs"></div>
  <div id="chat-input-row">
    <input id="chat-in" type="text" placeholder="Say something..." maxlength="80" autocomplete="off"/>
    <button id="chat-send">SEND</button>
  </div>
</div>

<!-- FINISH (single player only) -->
<div class="screen" id="fin">
  <h1>FINISH!</h1><p id="finsub"></p>
  <button id="btn-again">RACE AGAIN</button>
</div>

<!-- KICK / TIMEOUT OVERLAY -->
<div class="screen off" id="kick-screen">
  <div class="kick-box">
    <div id="kick-title">KICKED</div>
    <div id="kick-reason"></div>
    <div id="kick-timer">Returning in 30s...</div>
    <div id="kick-progress-wrap"><div id="kick-progress"></div></div>
  </div>
</div>

<!-- THREE.JS from reliable CDN -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ---- verify THREE loaded ----
if(typeof THREE === 'undefined'){
  document.body.innerHTML='<div style="color:#00ffe7;font-family:monospace;padding:40px;font-size:18px">ERROR: THREE.js failed to load.<br>Check your internet connection.</div>';
  throw new Error('THREE not loaded');
}

// =============================================
// FIREBASE REST
// =============================================
var FB='https://apex-drifter-official-default-rtdb.firebaseio.com';
var isMP=false,myId='',lastSync=0;
var remotes={};

function fbWrite(d){
  fetch(FB+'/apexdrift/players/'+myId+'.json',{
    method:'PUT',headers:{'Content-Type':'application/json'},
    body:JSON.stringify(d),keepalive:true
  }).catch(function(){});
}
function fbDel(){
  try{navigator.sendBeacon(FB+'/apexdrift/players/'+myId+'.json','null');}catch(e){}
  fetch(FB+'/apexdrift/players/'+myId+'.json',{method:'DELETE',keepalive:true}).catch(function(){});
}

// SSE - Firebase push streams (INSTANT, no polling)
var playerSSE=null,chatSSE=null,adminSSE=null;

function startSSE(){
  stopSSE();

  // Players - instant position updates
  playerSSE=new EventSource(FB+'/apexdrift/players.json');
  playerSSE.addEventListener('put',function(e){
    try{
      var p=JSON.parse(e.data);
      if(p.data) handleRemotes(p.data);
    }catch(x){}
  });
  playerSSE.addEventListener('patch',function(e){
    try{
      var p=JSON.parse(e.data);
      if(!p.data) return;
      // Build merged data from existing remotes + patch
      var merged={};
      Object.keys(remotes).forEach(function(id){merged[id]=remotes[id]._raw||{};});
      Object.keys(p.data).forEach(function(id){
        if(p.data[id]===null) delete merged[id];
        else merged[id]=p.data[id];
      });
      handleRemotes(merged);
    }catch(x){}
  });
  playerSSE.onerror=function(){ setTimeout(function(){if(isMP&&!playerSSE)startSSE();},1500); };

  // Chat - instant messages
  chatSSE=new EventSource(FB+'/apexdrift/chat.json');
  chatSSE.addEventListener('put',function(e){
    try{
      var p=JSON.parse(e.data);
      if(p.path==='/'&&p.data){
        // Initial snapshot - just update lastChatTs, don't show old msgs
        var vals=Object.values(p.data);
        var maxTs=vals.reduce(function(m,x){return(x&&x.ts>m)?x.ts:m;},0);
        if(maxTs) lastChatTs=maxTs;
        return;
      }
      // Single new message pushed
      var m=p.data;
      if(!m||!m.ts||m.ts<=lastChatTs) return;
      lastChatTs=m.ts;
      if(m.id===myId) return;
      addChatMessage(m,false);
    }catch(x){}
  });
  chatSSE.addEventListener('patch',function(e){
    try{
      var p=JSON.parse(e.data);
      if(!p.data) return;
      Object.keys(p.data).forEach(function(k){
        var m=p.data[k];
        if(!m||!m.ts||m.ts<=lastChatTs) return;
        lastChatTs=m.ts;
        if(m.id===myId) return;
        addChatMessage(m,false);
      });
    }catch(x){}
  });

  // mod channel
  adminSSE=new EventSource(FB+'/apexdrift/admin.json');
  adminSSE.addEventListener('put',function(e){
    try{
      var p=JSON.parse(e.data);
      if(p.path==='/'&&p.data){
        // Mark all existing as seen
        Object.keys(p.data).forEach(function(k){
          var cmd=p.data[k];if(cmd&&cmd.ts)lastAdminTs=Math.max(lastAdminTs,cmd.ts);
        });
        return;
      }
      var cmd=p.data;
      if(!cmd||!cmd.ts||cmd.ts<=lastAdminTs) return;
      lastAdminTs=cmd.ts;
      if(cmd.type==='ban'&&cmd.target===playerName) doBanKick(cmd.reason,cmd.by);
      if(cmd.type==='speed'&&cmd.by!==playerName){carSpdMS=cmd.kmh/3.6;carSpd=carSpdMS/277.8;}
    }catch(x){}
  });
}

function stopSSE(){
  if(playerSSE){playerSSE.close();playerSSE=null;}
  if(chatSSE){chatSSE.close();chatSSE=null;}
  if(adminSSE){adminSSE.close();adminSSE=null;}
}
function stopPoll(){ stopSSE(); }

// =============================================
// DEVICE
// =============================================
var devType='laptop', viewMode='3p';

function doAutoDetect(){
  var ua=navigator.userAgent.toLowerCase();
  var touch=navigator.maxTouchPoints>1;
  var w=window.innerWidth,h=window.innerHeight;
  var isIpad=/ipad/.test(ua)||(/macintosh/.test(ua)&&touch)||(touch&&Math.min(w,h)>=768);
  var isPhone=/iphone/.test(ua)||(touch&&!isIpad);
  var det=isIpad?'ipad':isPhone?'phone':'laptop';
  var names={laptop:'Laptop / Desktop',ipad:'iPad',phone:'iPhone'};
  var el=document.getElementById('dp-detected');
  el.textContent='Detected: '+names[det];
  ['dpL','dpI','dpP'].forEach(function(id){
    var b=document.getElementById(id);
    b.classList.remove('detected');
  });
  var map={laptop:'dpL',ipad:'dpI',phone:'dpP'};
  document.getElementById(map[det]).classList.add('detected');
  return det;
}

function setDev(type){
  devType=type;
  document.getElementById('dp').classList.add('off');
  document.getElementById('menu').classList.remove('off');
  if(type==='phone'||type==='ipad'){
    document.getElementById('tui').style.display='block';
    viewMode='3p';
    document.getElementById('v3p').classList.add('on');
    document.getElementById('v1p').classList.remove('on');
  }
  if(type==='phone'){
    checkRot();
    window.addEventListener('resize',checkRot);
  }
}

function checkRot(){
  if(devType!=='phone')return;
  var portrait=window.innerHeight>window.innerWidth;
  portrait?document.getElementById('rot').classList.remove('off'):document.getElementById('rot').classList.add('off');
}

// Auto-detect silently on load
window.addEventListener('load',function(){doAutoDetect();});

// =============================================
// PALETTE + WIRING
// =============================================
var PALETTE=['#00ffe7','#ff3a5c','#ffc820','#a78bfa','#34d399','#f97316','#38bdf8','#fb923c'];
var transmission = 'auto';
var engineType = 'petrol'; // 'petrol' or 'electric'

function setSl(type, val){
  if(type === 'cam'){
    viewMode = val === '3p' ? '3p' : '1p';
    document.getElementById('sl-cam-3p').classList.toggle('sl-on', val==='3p');
    document.getElementById('sl-cam-1p').classList.toggle('sl-on', val==='1p');
    var hint = document.getElementById('ctrl-hint');
    if(hint) hint.textContent = val==='1p'
      ? 'W=Gas S=Brake A/D=Steer Space=Drift ESC=Pause | Click game to lock mouse'
      : 'W=Gas S=Brake A/D=Steer Space=Drift ESC=Pause';
  }
  if(type === 'eng'){
    engineType = val;
    document.getElementById('sl-eng-petrol').classList.toggle('sl-on', val==='petrol');
    document.getElementById('sl-eng-electric').classList.toggle('sl-on', val==='electric');
  }
  if(type === 'trns'){
    transmission = val;
    document.getElementById('sl-trns-auto').classList.toggle('sl-on', val==='auto');
    document.getElementById('sl-trns-manual').classList.toggle('sl-on', val==='manual');
    var hint = document.getElementById('ctrl-hint');
    if(hint && val === 'manual') hint.textContent = 'W=Gas S=Brake A/D=Steer UP/DOWN=Gear Space=Drift ESC=Pause';
    if(hint && val === 'auto')   hint.textContent = 'W=Gas S=Brake A/D=Steer Space=Drift ESC=Pause';
    // Show/hide gear UI
    var gh = document.getElementById('gear-hud');
    var tup = document.getElementById('tup');
    var tdn = document.getElementById('tdn');
    if(gh)  gh.style.opacity  = val==='manual' ? '1' : '0.3';
    if(tup) tup.style.display = val==='manual' ? '' : 'none';
    if(tdn) tdn.style.display = val==='manual' ? '' : 'none';
  }
}
var pickedColor=PALETTE[0];

PALETTE.forEach(function(col,i){
  var sw=document.createElement('div');
  sw.className='sw'+(i===0?' on':'');
  sw.style.background=col;
  sw.onclick=function(){document.querySelectorAll('.sw').forEach(function(s){s.classList.remove('on');});sw.classList.add('on');pickedColor=col;};
  document.getElementById('swatch').appendChild(sw);
});

// camera controlled by setSl() sliders
document.getElementById('btn-solo').onclick=function(){initAudio();launch(false);};
document.getElementById('iname').addEventListener('keydown',function(e){if(e.key==='Enter')launch(false);});
document.getElementById('btn-mp').onclick=function(){document.getElementById('menu').classList.add('off');document.getElementById('mpm').classList.remove('off');startMPConn();};
document.getElementById('btn-cxl').onclick=function(){document.getElementById('mpm').classList.add('off');document.getElementById('menu').classList.remove('off');};
document.getElementById('btn-conn').onclick=function(){initAudio();launch(true);};
document.getElementById('btn-res').onclick=function(){paused=false;document.getElementById('paus').classList.add('off');};
document.getElementById('btn-quit').onclick=function(){location.reload();};
document.getElementById('btn-again').onclick=function(){location.reload();};

function startMPConn(){
  var st=document.getElementById('mpst'),btn=document.getElementById('btn-conn');
  btn.disabled=true;st.className='';st.textContent='Connecting...';
  fetch(FB+'/apexdrift/players.json?shallow=true').then(function(r){
    if(!r.ok) throw new Error('HTTP '+r.status);
    isMP=true;st.className='ok';st.textContent='Connected! Ready.';btn.disabled=false;
  }).catch(function(e){
    st.className='err';
    st.textContent='Failed: '+e.message+'\n\nFix Firebase rules:\n{\n  "rules":{"apexdrift":{"players":{".read":true,".write":true}}}\n}';
    btn.textContent='RETRY';btn.disabled=false;
    btn.onclick=function(){btn.textContent='JOIN RACE';btn.onclick=function(){launch(true);};startMPConn();};
  });
}

// =============================================
// TRACK
// =============================================
var TW=200,TH=90,SEGS=200,RW=26;
var TPTS=[];
for(var _i=0;_i<=SEGS;_i++){var _t=(_i/SEGS)*Math.PI*2;TPTS.push(new THREE.Vector3(TW*Math.cos(_t),0,TH*Math.sin(_t)));}
function trkPt(u){var i=((u%1)+1)%1*(TPTS.length-1),lo=Math.floor(i),hi=Math.min(lo+1,TPTS.length-1);return TPTS[lo].clone().lerp(TPTS[hi],i-lo);}
function trkTan(u){var dt=.001;return trkPt(u+dt).sub(trkPt(((u-dt)%1+1)%1)).normalize();}
function trkNorm(u){var t=trkTan(u);return new THREE.Vector3(-t.z,0,t.x);}
var CP_COUNT=24,TOTAL_LAPS=3,CPS=[];
for(var _ci=0;_ci<CP_COUNT;_ci++)CPS.push({pos:trkPt(_ci/CP_COUNT)});
var innerEdge=[],outerEdge=[];
for(var _ei=0;_ei<=SEGS;_ei++){var _u=_ei/SEGS,_n=trkNorm(_u),_p=trkPt(_u);innerEdge.push({x:_p.x-_n.x*(RW/2),z:_p.z-_n.z*(RW/2)});outerEdge.push({x:_p.x+_n.x*(RW/2),z:_p.z+_n.z*(RW/2)});}
function buildWallSegs(pts){var s=[];for(var i=0;i<pts.length-1;i++){var a=pts[i],b=pts[i+1],dx=b.x-a.x,dz=b.z-a.z,len=Math.sqrt(dx*dx+dz*dz);if(len>.01)s.push({ax:a.x,az:a.z,bx:b.x,bz:b.z,len});}return s;}
var wallsI=[],wallsO=[];

// =============================================
// THREE INIT
// =============================================
var scene,camera,renderer;
function initThree(){
  scene=new THREE.Scene();scene.background=new THREE.Color(0x080b18);scene.fog=new THREE.FogExp2(0x080b18,.004);
  renderer=new THREE.WebGLRenderer({canvas:document.getElementById('c'),antialias:true,powerPreference:'high-performance'});
  renderer.setPixelRatio(Math.min(window.devicePixelRatio,1.5));renderer.setSize(innerWidth,innerHeight);
  camera=new THREE.PerspectiveCamera(70,innerWidth/innerHeight,.1,1400);
  scene.add(new THREE.AmbientLight(0x2a3060,2.2));
  var sun=new THREE.DirectionalLight(0xfff0dd,1.8);sun.position.set(100,140,80);scene.add(sun);
  var pl1=new THREE.PointLight(0x00ffe7,1,160);pl1.position.set(-TW,20,0);scene.add(pl1);
  var pl2=new THREE.PointLight(0xff3a5c,.8,160);pl2.position.set(TW,20,0);scene.add(pl2);
  var sv=[];for(var i=0;i<500;i++)sv.push((Math.random()-.5)*2200,Math.random()*500+80,(Math.random()-.5)*2200);
  var sg=new THREE.BufferGeometry();sg.setAttribute('position',new THREE.Float32BufferAttribute(sv,3));
  scene.add(new THREE.Points(sg,new THREE.PointsMaterial({color:0xffffff,size:.7})));
  window.addEventListener('resize',function(){camera.aspect=innerWidth/innerHeight;camera.updateProjectionMatrix();renderer.setSize(innerWidth,innerHeight);});
}

function buildTrack(){
  var inner=[],outer=[];
  for(var i=0;i<=SEGS;i++){var u=i/SEGS,n=trkNorm(u),p=trkPt(u);inner.push(p.clone().addScaledVector(n,-RW/2));outer.push(p.clone().addScaledVector(n,RW/2));}
  var rv=[],ruv=[],ri=[];
  for(var i=0;i<inner.length;i++){rv.push(inner[i].x,0,inner[i].z,outer[i].x,0,outer[i].z);ruv.push(0,i/inner.length,1,i/inner.length);if(i<inner.length-1){var b=i*2;ri.push(b,b+1,b+2,b+1,b+3,b+2);}}
  var rg=new THREE.BufferGeometry();rg.setAttribute('position',new THREE.Float32BufferAttribute(rv,3));rg.setAttribute('uv',new THREE.Float32BufferAttribute(ruv,2));rg.setIndex(ri);rg.computeVertexNormals();
  scene.add(new THREE.Mesh(rg,new THREE.MeshLambertMaterial({color:0x181b2e})));
  for(var i=0;i<SEGS;i+=4){var u=i/SEGS,p=trkPt(u),tan=trkTan(u);var d=new THREE.Mesh(new THREE.PlaneGeometry(.5,3.5),new THREE.MeshBasicMaterial({color:0xffffff,opacity:.18,transparent:true}));d.rotation.x=-Math.PI/2;d.rotation.z=-Math.atan2(tan.x,tan.z);d.position.set(p.x,.01,p.z);scene.add(d);}
  var mR=new THREE.MeshLambertMaterial({color:0xff2244}),mW=new THREE.MeshLambertMaterial({color:0xffffff});
  [inner,outer].forEach(function(pts){for(var i=0;i<pts.length-1;i++){var a=pts[i],b=pts[i+1],len=a.distanceTo(b);if(len<.01)return;var wall=new THREE.Mesh(new THREE.BoxGeometry(len,.9,.1),Math.floor(i/3)%2===0?mR:mW);wall.position.set((a.x+b.x)/2,.65,(a.z+b.z)/2);wall.rotation.y=Math.atan2(b.x-a.x,b.z-a.z);scene.add(wall);}});
  wallsI=buildWallSegs(innerEdge);wallsO=buildWallSegs(outerEdge);
  var sp=trkPt(0),st=trkTan(0);var sf=new THREE.Mesh(new THREE.PlaneGeometry(RW,1.6),new THREE.MeshBasicMaterial({color:0xffffff,opacity:.85,transparent:true}));sf.rotation.x=-Math.PI/2;sf.rotation.z=-Math.atan2(st.x,st.z);sf.position.set(sp.x,.015,sp.z);scene.add(sf);
  var gnd=new THREE.Mesh(new THREE.PlaneGeometry(1600,1600),new THREE.MeshLambertMaterial({color:0x060910}));gnd.rotation.x=-Math.PI/2;gnd.position.y=-.1;scene.add(gnd);
  [[-TH-28,80],[TH+28,80]].forEach(function(zw){var gs=new THREE.Mesh(new THREE.BoxGeometry(zw[1],16,10),new THREE.MeshLambertMaterial({color:0x101520}));gs.position.set(0,8,zw[0]);scene.add(gs);});
  [[-TW-12,0],[TW+12,0],[0,-TH-22],[0,TH+22]].forEach(function(xz){var tw=new THREE.Mesh(new THREE.BoxGeometry(1.2,34,1.2),new THREE.MeshLambertMaterial({color:0x1a2240}));tw.position.set(xz[0],17,xz[1]);scene.add(tw);var pl=new THREE.PointLight(0xfff5e0,1,180);pl.position.set(xz[0],34,xz[1]);scene.add(pl);});
}

// =============================================
// CAR
// =============================================
function buildCar(hex){
  var g=new THREE.Group();
  var bm=new THREE.MeshLambertMaterial({color:new THREE.Color(hex)});
  var dm=new THREE.MeshLambertMaterial({color:0x111111});
  function mk(geo,mat,x,y,z){var m=new THREE.Mesh(geo,mat);m.position.set(x,y,z);g.add(m);return m;}
  mk(new THREE.BoxGeometry(2.1,.68,4.4),bm,0,.45,0);
  mk(new THREE.BoxGeometry(1.8,.55,2.1),bm,0,1.02,-.2);
  mk(new THREE.BoxGeometry(1.7,.05,.9),bm,0,1.18,-2.0);
  [[1.12,1.4],[1.12,-1.4],[-1.12,1.4],[-1.12,-1.4]].forEach(function(wz){mk(new THREE.BoxGeometry(.32,.88,.88),dm,wz[0],.44,wz[1]);});
  mk(new THREE.BoxGeometry(.28,.16,.08),new THREE.MeshBasicMaterial({color:0xffffff}),.65,.52,2.22);
  mk(new THREE.BoxGeometry(.28,.16,.08),new THREE.MeshBasicMaterial({color:0xffffff}),-.65,.52,2.22);
  var tlm=new THREE.MeshBasicMaterial({color:0xff1a1a});
  mk(new THREE.BoxGeometry(.28,.16,.08),tlm,.65,.52,-2.22);mk(new THREE.BoxGeometry(.28,.16,.08),tlm,-.65,.52,-2.22);
  var rvm=new THREE.MeshBasicMaterial({color:0xffffff,transparent:true,opacity:0});
  var r1=mk(new THREE.BoxGeometry(.2,.12,.07),rvm.clone(),.32,.48,-2.24);
  var r2=mk(new THREE.BoxGeometry(.2,.12,.07),rvm.clone(),-.32,.48,-2.24);
  g.userData.revL=[r1,r2];
  var brm=new THREE.MeshBasicMaterial({color:0xff4400,transparent:true,opacity:0});
  var b1=mk(new THREE.BoxGeometry(.32,.2,.06),brm.clone(),.65,.52,-2.25);
  var b2=mk(new THREE.BoxGeometry(.32,.2,.06),brm.clone(),-.65,.52,-2.25);
  g.userData.brkL=[b1,b2];
  var glow=new THREE.PointLight(new THREE.Color(hex),1.5,8);glow.position.set(0,-.1,0);g.add(glow);
  return g;
}
function makeLabel(name,hex){
  var cv=document.createElement('canvas');cv.width=256;cv.height=56;
  var ctx=cv.getContext('2d');ctx.fillStyle=hex;ctx.font='bold 26px sans-serif';ctx.textAlign='center';ctx.fillText(name,128,38);
  var sp=new THREE.Sprite(new THREE.SpriteMaterial({map:new THREE.CanvasTexture(cv),transparent:true,depthTest:false}));sp.scale.set(4,1,1);return sp;
}

// =============================================
// COLLISION
// =============================================
var CAR_R=1.9,BNCE=0.28,CCOL=2.8;
function resolveWalls(){
  var hit=false;
  var walls=wallsI.concat(wallsO);
  for(var wi=0;wi<walls.length;wi++){
    var w=walls[wi];
    var ex=carPos.x-w.ax,ez=carPos.z-w.az,wdx=w.bx-w.ax,wdz=w.bz-w.az,wux=wdx/w.len,wuz=wdz/w.len;
    var t=ex*wux+ez*wuz;if(t<0||t>w.len)continue;
    var cpx=w.ax+wux*t,cpz=w.az+wuz*t,dx=carPos.x-cpx,dz=carPos.z-cpz,dist=Math.sqrt(dx*dx+dz*dz);
    if(dist<CAR_R&&dist>.001){carPos.x+=dx/dist*(CAR_R-dist);carPos.z+=dz/dist*(CAR_R-dist);carSpdMS*=BNCE;carSpd=carSpdMS/277.8;hit=true;}
  }
  if(hit){var hf=document.getElementById('hf');hf.style.opacity='1';setTimeout(function(){hf.style.opacity='0';},80);playClunk();}
}
function resolveCarCol(){
  if(!isMP)return;
  Object.keys(remotes).forEach(function(id){
    var r=remotes[id];
    var dx=carPos.x-r.mesh.position.x,dz=carPos.z-r.mesh.position.z,dist=Math.sqrt(dx*dx+dz*dz);
    if(dist<CCOL&&dist>.01){var ov=CCOL-dist,nx=dx/dist,nz=dz/dist;carPos.x+=nx*ov*.6;carPos.z+=nz*ov*.6;carSpdMS*=0.82;carSpd=carSpdMS/277.8;}
  });
}

// =============================================
// SMOKE
// =============================================
var smoke=[];
function spawnSmoke(pos){
  var m=new THREE.Mesh(new THREE.SphereGeometry(.28,4,4),new THREE.MeshBasicMaterial({color:0x888888,transparent:true,opacity:.38}));
  m.position.set(pos.x+(Math.random()-.5)*1.4,.28,pos.z+(Math.random()-.5)*1.4);m._life=1;scene.add(m);smoke.push(m);
}
function tickSmoke(){
  for(var i=smoke.length-1;i>=0;i--){var s=smoke[i];s._life-=.024;s.material.opacity=s._life*.38;s.scale.setScalar(1+(1-s._life)*4);s.position.y+=.011;if(s._life<=0){scene.remove(s);smoke.splice(i,1);}}
}

// =============================================
// ENGINE - 6 GEARS, G6=1000KM/H
// =============================================
var GTOP_PETROL=[22,60,120,180,240,277.8];
var GACC_PETROL=[45,38,30,22,16,11];
var GTOP_ELECTRIC=[50,120,200,277.8,277.8,277.8]; // electric torque = instant, same top
var GACC_ELECTRIC=[90,70,50,35,22,14]; // electric accel = WAY faster off the line
var GTOP=GTOP_PETROL;
var GACC=GACC_PETROL;
var RPM_IDLE=900,RPM_MAX=7200,SHIFT_MS=260;
var gear=0,rpm=RPM_IDLE,clutch=0,shiftT=0;
var carSpdMS=0,carSpd=0;

function shiftGear(d){
  var next=Math.max(0,Math.min(5,gear+d));if(next===gear)return;
  gear=next;clutch=1;shiftT=SHIFT_MS;
  document.getElementById('gear-val').textContent=gear+1;playShiftClick();
  var gf=document.getElementById('gf');
  gf.textContent=(d>0?'  ':'  ')+(gear+1);gf.style.color=d>0?'var(--neon)':'var(--gold)';
  gf.style.opacity='1';setTimeout(function(){gf.style.opacity='0';},380);
}

// =============================================
// INPUT
// =============================================
var keys={};
document.addEventListener('keydown',function(e){
  keys[e.key]=true;
  if(e.key===' ')e.preventDefault();
  if(e.key==='Escape'&&gameOn&&!done){paused=!paused;document.getElementById('paus').classList.toggle('off',!paused);}
  if(gameOn&&!done&&!paused&&transmission==='manual'){if(e.key==='ArrowUp')shiftGear(1);if(e.key==='ArrowDown')shiftGear(-1);}
});
document.addEventListener('keyup',function(e){keys[e.key]=false;});
var mouseX=0;
document.addEventListener('mousemove',function(e){if(gameOn&&!done&&!paused)mouseX+=e.movementX;});
document.getElementById('c').addEventListener('click',function(){
  if(audioCtx&&audioCtx.state==='suspended') audioCtx.resume();
  if(viewMode==='1p'&&gameOn&&!done) document.getElementById('c').requestPointerLock();
});
var tiltX=0;
window.addEventListener('deviceorientation',function(e){if(e.gamma!==null)tiltX=Math.max(-45,Math.min(45,e.gamma));},{passive:true});

// Touch joystick
var joyAct=false,joyCX=0;
var jEl=document.getElementById('jl'),jkEl=document.getElementById('jk');
var swSvg = document.getElementById('sw-svg');
var wheelRot = 0, wheelVel = 0, wheelTarget = 0;

// Exact same spring physics as the widget preview
function updateWheelVisual(){
  if(!swSvg) return;
  // Target follows steer value (keyboard) or joystick
  wheelTarget = joyAct ? joyCX * 135 : (steer / MAX_ST) * 135;
  var force = (wheelTarget - wheelRot) * 0.3;
  wheelVel = (wheelVel + force) * 0.75;
  wheelRot += wheelVel;
  // Hard clamp
  if(wheelRot >  135){ wheelRot =  135; wheelVel = 0; }
  if(wheelRot < -135){ wheelRot = -135; wheelVel = 0; }
  swSvg.style.transform = 'rotate('+wheelRot.toFixed(2)+'deg)';
}

var joyStartAngle=0, joyStartRot=0;

function getTouchAngle(e){
  var r=jEl.getBoundingClientRect();
  var cx=r.left+r.width/2,cy=r.top+r.height/2;
  var t=e.touches[0];
  return Math.atan2(t.clientY-cy,t.clientX-cx)*180/Math.PI;
}

jEl.addEventListener('touchstart',function(e){
  e.preventDefault();
  joyAct=true; wheelVel=0;
  joyStartAngle=getTouchAngle(e);
  joyStartRot=wheelRot;
  var r=jEl.getBoundingClientRect();
  jEl._cx=r.left+r.width/2; jEl._cy=r.top+r.height/2;
},{passive:false});

jEl.addEventListener('touchmove',function(e){
  e.preventDefault();
  if(!joyAct)return;
  var delta=getTouchAngle(e)-joyStartAngle;
  while(delta>180)delta-=360;
  while(delta<-180)delta+=360;
  var tgt=Math.max(-135,Math.min(135,joyStartRot+delta));
  joyCX=tgt/135;
},{passive:false});

jEl.addEventListener('touchend',function(e){
  e.preventDefault();
  joyAct=false; joyCX=0;
},{passive:false});

jEl.addEventListener('touchcancel',function(e){
  e.preventDefault();
  joyAct=false; joyCX=0;
},{passive:false});


function tb(id,k){var el=document.getElementById(id);if(!el)return;el.addEventListener('touchstart',function(e){e.preventDefault();keys[k]=true;},{passive:false});el.addEventListener('touchend',function(e){e.preventDefault();keys[k]=false;},{passive:false});}
tb('tgas','w');tb('tbrk','s');tb('tdft',' ');
document.getElementById('tup').addEventListener('touchstart',function(e){e.preventDefault();if(gameOn&&!done)shiftGear(1);},{passive:false});
document.getElementById('tdn').addEventListener('touchstart',function(e){e.preventDefault();if(gameOn&&!done)shiftGear(-1);},{passive:false});

// =============================================
// GAME STATE
// =============================================
var carPos=new THREE.Vector3();
var carAngle=0,velAngle=0,steer=0,driftAmt=0;
var lap=1,lastCP=-1,cpTotal=0,gameOn=false,done=false,paused=false,startTime=0;
var myCarMesh=null,bounce=0,bounceV=0,camX=0,camZ=0,camY=0,camRdy=false;
var fpPitch=0,fpRoll=0,fwdKey=false,bkKey=false;
var playerName='',playerColor='';
var fpsF=0,fpsLast=performance.now();
var MAX_ST=0.85,ST_RTN=0.10,BASE_TURN=0.044,GRIP=0.87,DGRIP=0.50;
var mmCtx=document.getElementById('mmc').getContext('2d');
function aDiff(a,b){var d=a-b;while(d>Math.PI)d-=Math.PI*2;while(d<-Math.PI)d+=Math.PI*2;return d;}

// =============================================
// LAUNCH
// =============================================
function launch(mp){
  playerName=document.getElementById('iname').value.trim()||'Driver';
  playerColor=pickedColor;
  // Set gear arrays based on engine type
  if(engineType==='electric'){
    GTOP=GTOP_ELECTRIC; GACC=GACC_ELECTRIC;
  } else {
    GTOP=GTOP_PETROL; GACC=GACC_PETROL;
  }
  document.getElementById('menu').classList.add('off');
  document.getElementById('mpm').classList.add('off');
  initThree();buildTrack();
  var sp=trkPt(.005);carPos.set(sp.x,.4,sp.z);
  var st=trkTan(.005);carAngle=Math.atan2(st.x,st.z);velAngle=carAngle;
  myCarMesh=buildCar(playerColor);myCarMesh.position.copy(carPos);myCarMesh.rotation.y=carAngle;
  var lbl=makeLabel(playerName,playerColor);lbl.position.set(0,2.4,0);myCarMesh.add(lbl);scene.add(myCarMesh);
  document.getElementById('hud').style.display='block';
  document.getElementById('lap-val').textContent='1 / '+TOTAL_LAPS;
  if(mp&&isMP){setupMP();document.getElementById('mpdot').style.display='block';}
  countdown(function(){gameOn=true;startTime=Date.now();});
  requestAnimationFrame(loop);
}
function countdown(cb){
  var el=document.getElementById('cd');
  function show(t,cc){el.className='show'+(cc?' '+cc:'');el.textContent=t;setTimeout(function(){el.className='';},680);}
  show('3');playCountdownBeep(false);
  setTimeout(function(){show('2');playCountdownBeep(false);},1000);
  setTimeout(function(){show('1');playCountdownBeep(false);},2000);
  setTimeout(function(){show('GO!','go');playCountdownBeep(true);cb();},3000);
}

// =============================================
// PHYSICS
// =============================================
function tick(DT){
  if(!gameOn||done||paused)return;
  fwdKey=!!(keys['w']||keys['W']);
  bkKey=!!(keys['s']||keys['S']);
  var kL=!!(keys['a']||keys['A']),kR=!!(keys['d']||keys['D']);
  var hbk=!!(keys[' ']||keys['Shift']||keys['ShiftLeft']||keys['ShiftRight']);
  if(shiftT>0){shiftT-=DT*1000;clutch=Math.max(0,shiftT/SHIFT_MS);}else clutch=0;
  var topMS=GTOP[gear],acc=GACC[gear]*(1-clutch*.65),REV_MAX=25;
  if(fwdKey){carSpdMS=carSpdMS<0?Math.min(0,carSpdMS+acc*3*DT):Math.min(carSpdMS+acc*DT,topMS);}
  else if(bkKey){carSpdMS=carSpdMS>0?Math.max(0,carSpdMS-acc*2.5*DT):Math.max(carSpdMS-acc*.4*DT,-REV_MAX);}
  else{if(Math.abs(carSpdMS)<1)carSpdMS=0;else carSpdMS-=Math.sign(carSpdMS)*5*DT;}
  if(carSpdMS>0)carSpdMS=Math.max(0,carSpdMS-.00035*carSpdMS*carSpdMS*DT);
  carSpd=carSpdMS/277.8;
  var rpmT=RPM_IDLE+Math.min(1,Math.abs(carSpdMS)/Math.max(topMS,1))*(RPM_MAX-RPM_IDLE)*(1+clutch*.4);
  rpm+=(Math.min(RPM_MAX,rpmT)-rpm)*.14;rpm=Math.max(RPM_IDLE,Math.min(RPM_MAX,rpm));
  // AUTO TRANSMISSION: shift up at 90% RPM, shift down at 35% RPM
  if(transmission==='auto' && fwdKey && clutch<0.1){
    if(rpm > RPM_MAX*0.90 && gear < 5) shiftGear(1);
    else if(rpm < RPM_MAX*0.35 && gear > 0 && carSpdMS > 2) shiftGear(-1);
  }
  // Mouse delta this frame (consumed each frame)
  var mouseDelta = mouseX; mouseX = 0;
  if(joyAct){
    // Smooth interpolation - no snap when reversing direction
    var joyTarget = joyCX * MAX_ST;
    steer += (joyTarget - steer) * 0.2;
  } else {
    // Keyboard steering
    if(kL) steer -= .07;
    else if(kR) steer += .07;
    else steer *= (1 - ST_RTN * 2); // faster return to center when no key
    // Tilt steering (mobile)
    steer += tiltX * .0005;
    // Clamp
    steer = Math.max(-MAX_ST, Math.min(MAX_ST, steer));
  }
  // Turn car (only when moving)
  if(Math.abs(carSpdMS) > 0.5){
    var sf = 1/(1 + Math.abs(carSpdMS)*.016);
    carAngle -= BASE_TURN*(steer/MAX_ST)*sf*(1+driftAmt*.2)*Math.sign(carSpdMS);
  }
  // Sync vel direction to heading (grip) - only when moving
  var isDrift = hbk && Math.abs(carSpdMS) > 3;
  if(Math.abs(carSpdMS) > 0.3){
    var grip = isDrift ? DGRIP : GRIP;
    velAngle += aDiff(carAngle, velAngle) * (1-grip);
    driftAmt += (Math.abs(aDiff(velAngle,carAngle)) - driftAmt) * .10;
  } else {
    // Stopped - snap vel to heading, no drift
    velAngle = carAngle;
    driftAmt = 0;
  }
  if(isDrift) carSpdMS = carSpdMS>0 ? Math.max(0,carSpdMS-driftAmt*2*DT) : Math.min(0,carSpdMS+driftAmt*2*DT);
  carPos.x+=Math.sin(velAngle)*carSpd;carPos.z+=Math.cos(velAngle)*carSpd;carPos.y=.4;
  resolveWalls();resolveCarCol();
  if(myCarMesh){
    myCarMesh.position.copy(carPos);myCarMesh.rotation.y=carAngle;
    myCarMesh.rotation.z+=((-steer*.07*Math.min(Math.abs(carSpdMS)/60,1))-myCarMesh.rotation.z)*.12;
    var rev=carSpdMS<-.5,brk=bkKey&&carSpdMS>.5;
    if(myCarMesh.userData.revL)myCarMesh.userData.revL.forEach(function(m){m.material.opacity=rev?1:0;});
    if(myCarMesh.userData.brkL)myCarMesh.userData.brkL.forEach(function(m){m.material.opacity=brk?1:0;});
  }
  if((isDrift||driftAmt>.08)&&Math.random()<.5)spawnSmoke(carPos);
  tickSmoke();
  var kmh=Math.round(Math.abs(carSpdMS)*3.6);
  document.getElementById('spd').textContent=kmh;
  var rb=document.getElementById('rpmb');rb.style.width=Math.min(100,rpm/RPM_MAX*100)+'%';
  rb.style.background=rpm>RPM_MAX*.88?'linear-gradient(90deg,var(--hot),#f60)':'linear-gradient(90deg,var(--neon),var(--gold))';
  // Wheel visual updated every frame via spring physics
  updateWheelVisual();
  updateAudio(DT);
  fpsF++;var fNow=performance.now();
  if(fNow-fpsLast>600){document.getElementById('fps').textContent=Math.round(fpsF*1000/(fNow-fpsLast))+' FPS';fpsF=0;fpsLast=fNow;}
  checkCPs();checkWW();
  if(isMP)syncFB();
}

// =============================================
// CAMERA
// =============================================
function updateCam(){
  if(viewMode==='3p'){
    var tx=carPos.x-Math.sin(carAngle)*12,tz=carPos.z-Math.cos(carAngle)*12,ty=carPos.y+5;
    if(!camRdy){camX=tx;camZ=tz;camY=ty;camRdy=true;}
    camX+=(tx-camX)*.09;camZ+=(tz-camZ)*.09;camY+=(ty-camY)*.09;
    bounceV+=(0-bounce)*.2;bounce+=bounceV;if(Math.abs(carSpdMS)>5)bounceV+=(Math.random()-.5)*(Math.abs(carSpdMS)/300)*.003;bounceV*=.9;
    camera.position.set(camX,camY+bounce,camZ);
    camera.lookAt(carPos.x+Math.sin(carAngle)*3,carPos.y+1,carPos.z+Math.cos(carAngle)*3);
  }else{
    var ex=carPos.x+Math.sin(carAngle)*.5,ez=carPos.z+Math.cos(carAngle)*.5;
    bounceV+=(0-bounce)*.18;bounce+=bounceV;if(Math.abs(carSpdMS)>1)bounceV+=(Math.random()-.5)*(Math.abs(carSpdMS)/138.9)*.005;bounceV*=.88;
    camera.position.set(ex,carPos.y+1.1+bounce,ez);
    var tP=fwdKey?-.04:bkKey?.05:0;fpPitch+=(tP-fpPitch)*.06;
    var tR=-steer*.04*Math.min(Math.abs(carSpdMS)/138.9,1);fpRoll+=(tR-fpRoll)*.08;
    camera.rotation.order='YXZ';camera.rotation.y=carAngle+Math.PI;camera.rotation.x=fpPitch;camera.rotation.z=fpRoll;
  }
}

// =============================================
// CHECKPOINTS
// =============================================
function checkCPs(){
  for(var i=0;i<CPS.length;i++){
    if(carPos.distanceTo(CPS[i].pos)<22&&i!==lastCP&&i===(lastCP+1)%CP_COUNT){
      lastCP=i;cpTotal++;
      if(i===0&&cpTotal>1){if(lap<TOTAL_LAPS){lap++;document.getElementById('lap-val').textContent=lap+' / '+TOTAL_LAPS;}else finish();}
    }
  }
}
var wwT=0;
function checkWW(){
  var tan=trkTan(lastCP<0?0:lastCP/CP_COUNT),dir=new THREE.Vector3(Math.sin(carAngle),0,Math.cos(carAngle)),ww=document.getElementById('ww');
  if(dir.dot(tan)<-.5&&carSpdMS>1.5){wwT++;if(wwT>35)ww.style.display='block';}else{wwT=0;ww.style.display='none';}
}
function finish(){
  done=true;
  if(document.pointerLockElement)document.exitPointerLock();
  if(isMP){
    // No finish screen in MP - just show a chat message and keep playing
    addSysMsg('You completed '+TOTAL_LAPS+' laps! Time: '+((Date.now()-startTime)/1000).toFixed(2)+'s');
    // Reset for another run
    lap=1; lastCP=-1; cpTotal=0; startTime=Date.now(); done=false;
    addSysMsg('Starting another lap! Race on!');
  } else {
    document.getElementById('finsub').textContent='Time: '+((Date.now()-startTime)/1000).toFixed(2)+'s';
    document.getElementById('fin').classList.add('show');
  }
}

// =============================================
// MINIMAP
// =============================================
function drawMM(){
  var W=112,H=112,sx=W/(TW*2+60),sz=H/(TH*2+60),cx=W/2,cz=H/2;
  mmCtx.clearRect(0,0,W,H);
  mmCtx.beginPath();TPTS.forEach(function(p,i){var mx=cx+p.x*sx,mz=cz+p.z*sz;i===0?mmCtx.moveTo(mx,mz):mmCtx.lineTo(mx,mz);});mmCtx.closePath();mmCtx.strokeStyle='rgba(255,255,255,.18)';mmCtx.lineWidth=5;mmCtx.stroke();
  Object.keys(remotes).forEach(function(id){var r=remotes[id];var mx=cx+r.mesh.position.x*sx,mz=cz+r.mesh.position.z*sz;mmCtx.beginPath();mmCtx.arc(mx,mz,4,0,Math.PI*2);mmCtx.fillStyle=r.color;mmCtx.fill();});
  var mx=cx+carPos.x*sx,mz=cz+carPos.z*sz;
  mmCtx.beginPath();mmCtx.arc(mx,mz,5,0,Math.PI*2);mmCtx.fillStyle='#00ffe7';mmCtx.fill();
  mmCtx.beginPath();mmCtx.moveTo(mx,mz);mmCtx.lineTo(mx+Math.sin(carAngle)*9,mz+Math.cos(carAngle)*9);mmCtx.strokeStyle='#00ffe7';mmCtx.lineWidth=2;mmCtx.stroke();
}

// =============================================
// MULTIPLAYER
// =============================================
function setupMP(){
  myId=Math.random().toString(36).slice(2,10);
  joinedAt = Date.now();
  window.addEventListener('beforeunload',function(){fbDel();});
  window.addEventListener('pagehide',function(){fbDel();});
  document.addEventListener('visibilitychange',function(){
    if(document.visibilityState==='hidden'){fbDel();stopPoll();}
    else if(isMP){ startSSE(); }
  });
  startSSE(); // single SSE connection replaces all polling
  startSessionTimer();
  cleanOldChat();
  // Show chat UI
  document.getElementById('chat-toggle').style.display = 'flex';
  addSysMsg('Joined the race! Type to chat with other drivers.');
}
function handleRemotes(data){
  if(!scene)return;
  var lb=[{id:myId,name:playerName,color:playerColor,lap:lap,cp:cpTotal}];
  Object.keys(data).forEach(function(id){
    var p=data[id];
    if(id===myId||!p||typeof p.x==='undefined')return;
    if(!remotes[id]){
      var mesh=buildCar(p.color||'#ff3a5c');mesh.position.set(p.x,.4,p.z);mesh.rotation.y=p.angle||0;
      var lbl=makeLabel(p.name||'Driver',p.color||'#ff3a5c');lbl.position.set(0,2.4,0);mesh.add(lbl);
      scene.add(mesh);remotes[id]={mesh:mesh,color:p.color||'#ff3a5c',name:p.name||'Driver',lap:1,cp:0,tx:p.x,tz:p.z,ta:p.angle||0,vx:0,vz:0,lastUp:Date.now(),srvT:p.t||Date.now(),_rawData:p};
      addSysMsg((p.name||'Driver')+' joined the race!');
    }else{
      var r=remotes[id];
      var dt=Math.max(.01,(p.t&&r.srvT)?(p.t-r.srvT)/1000:(Date.now()-r.lastUp)/1000);
      r.vx=(p.x-r.tx)/dt;r.vz=(p.z-r.tz)/dt;r.tx=p.x;r.tz=p.z;r.ta=p.angle||0;r.lap=p.lap||1;r.cp=p.cp||0;r.lastUp=Date.now();r.srvT=p.t||Date.now();r._rawData=p;
    }
    lb.push({id:id,name:p.name||'Driver',color:p.color||'#888',lap:p.lap||1,cp:p.cp||0});
  });
  Object.keys(remotes).forEach(function(id){if(!data[id]){addSysMsg((remotes[id].name||'A driver')+' left the race.');scene.remove(remotes[id].mesh);delete remotes[id];}});
  renderLB(lb);
  if(isMP) checkPlayerExpiry(data);
}
function updateRemotes(){
  if(!isMP)return;
  var now=Date.now();
  Object.keys(remotes).forEach(function(id){
    var r=remotes[id];
    if(now-r.lastUp>3000){scene.remove(r.mesh);delete remotes[id];return;}
    var el=(now-r.lastUp)/1000;
    r.mesh.position.x+=(r.tx+r.vx*el-r.mesh.position.x)*.5;
    r.mesh.position.z+=(r.tz+r.vz*el-r.mesh.position.z)*.5;
    r.mesh.position.y=.4;
    r.mesh.rotation.y+=aDiff(r.ta,r.mesh.rotation.y)*.4;
  });
}
function syncFB(){
  var now=Date.now();if(now-lastSync<16)return;lastSync=now; // ~60fps writes
  fbWrite({x:+carPos.x.toFixed(2),z:+carPos.z.toFixed(2),y:.4,angle:+carAngle.toFixed(3),lap:lap,cp:cpTotal,name:playerName,color:playerColor,gear:gear+1,kmh:Math.round(Math.abs(carSpdMS)*3.6),t:now,joinedAt:joinedAt});
}
function renderLB(players){
  players.sort(function(a,b){return b.lap-a.lap||b.cp-a.cp;});
  var list=document.getElementById('lb-list');list.innerHTML='';
  players.slice(0,6).forEach(function(p,i){var d=document.createElement('div');d.className='lbe';d.innerHTML='<span class="lp">'+(i+1)+'</span><span class="ld" style="background:'+p.color+'"></span><span class="ln">'+p.name+'</span><span class="ll">L'+p.lap+'</span>';list.appendChild(d);});
}


// =============================================
// LIVE CHAT (Firebase REST)
// =============================================
var chatPollTmr = null;
var chatOpen = false;
var unreadCount = 0;
var lastChatTs = Date.now();
var chatMsgsEl = document.getElementById('chat-msgs');
var chatInEl   = document.getElementById('chat-in');
var chatToggle = document.getElementById('chat-toggle');
var chatBox    = document.getElementById('chat-box');
var chatBadge  = document.getElementById('chat-badge');

function toggleChat(){
  chatOpen = !chatOpen;
  chatBox.style.display = chatOpen ? 'flex' : 'none';
  chatToggle.style.display = 'flex';
  if(chatOpen){
    unreadCount = 0;
    chatBadge.style.display = 'none';
    chatBadge.textContent = '';
    chatMsgsEl.scrollTop = chatMsgsEl.scrollHeight;
    chatInEl.focus();
  }
}

chatToggle.addEventListener('click', toggleChat);
document.getElementById('chat-send').addEventListener('click', sendChatMsg);
chatInEl.addEventListener('keydown', function(e){
  if(e.key === 'Enter'){ e.preventDefault(); sendChatMsg(); }
  e.stopPropagation(); // prevent game keys
});
chatInEl.addEventListener('keyup', function(e){ e.stopPropagation(); });

var ADMINS = ['TDVADMIN','PixlectADMIN'];
function isAdmin(){ return ADMINS.indexOf(playerName) !== -1; }

function sendChatMsg(){
  var txt = chatInEl.value.trim();
  if(!txt) return;
  if(!isMP || !myId){
    addSysMsg('Connect to multiplayer to chat!');
    return;
  }

  // Admin commands
  if(txt.startsWith('/') && isAdmin()){
    handleAdminCmd(txt);
    chatInEl.value = '';
    return;
  }

  var now = Date.now();
  var msg = { n:playerName, c:playerColor, t:txt, ts:now, id:myId };
  chatInEl.value = '';
  addChatMessage(msg, true);
  lastChatTs = now;
  fetch(FB+'/apexdrift/chat.json', {
    method:'POST',
    headers:{'Content-Type':'application/json'},
    body: JSON.stringify(msg)
  }).catch(function(){});
}

function handleAdminCmd(txt){
  var parts = txt.split(' ');
  var cmd=parts[0].toLowerCase(),target=parts[1]||'',reason=parts.slice(2).join(' ')||'No reason given';
  if(cmd==='/ban'){
    if(!target){ return; }
    addSysMsg('Banned: '+target);
    // Write ban to Firebase
    var banMsg = {
      type:'ban',
      target: target,
      reason: reason,
      by: playerName,
      ts: Date.now()
    };
    fetch(FB+'/apexdrift/admin.json', {
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body: JSON.stringify(banMsg)
    }).catch(function(){});
  }
  else if(cmd==='/speed'){
    var kmh = parseFloat(target);
    if(isNaN(kmh)){ addSysMsg('Usage: /speed [kmh]'); return; }
    // Set speed for all players via admin node
    var speedMsg = { type:'speed', kmh:kmh, by:playerName, ts:Date.now() };
    fetch(FB+'/apexdrift/admin.json', {
      method:'POST',
      headers:{'Content-Type':'application/json'},
      body: JSON.stringify(speedMsg)
    }).catch(function(){});
    // silent
  }
  else {
    return; // silently ignore
  }
}

// Admin handled by SSE stream above
var lastAdminTs = Date.now();

function showKickScreen(title, reason){
  document.getElementById('kick-screen').classList.remove('off');
  document.getElementById('kick-title').textContent = title;
  document.getElementById('kick-reason').textContent = reason;
  var countdown = 30;
  document.getElementById('kick-timer').textContent = 'Returning in ' + countdown + 's...';
  var prog = document.getElementById('kick-progress');
  prog.style.width = '100%';
  var iv = setInterval(function(){
    countdown--;
    document.getElementById('kick-timer').textContent = 'Returning in ' + countdown + 's...';
    prog.style.width = ((countdown/30)*100) + '%';
    if(countdown <= 0){ clearInterval(iv); location.reload(); }
  }, 1000);
  // Clean up Firebase presence
  fbDel(); stopPoll();
  if(sessionTimer) clearInterval(sessionTimer);
}

function doBanKick(reason, by){
  gameOn = false;
  gameOn = false;
  showKickScreen('BANNED', reason + '\n\nChange your name to rejoin.');
}


function addChatMessage(msg, isMine){
  var div = document.createElement('div');
  div.className = 'chat-msg' + (isMine ? ' cm-mine' : '');
  var nameSpan = '<span class="cm-name" style="color:'+msg.c+'">' + escHtml(msg.n) + ':</span>';
  div.innerHTML = nameSpan + escHtml(msg.t);
  chatMsgsEl.appendChild(div);
  // Keep last 60 messages
  while(chatMsgsEl.children.length > 60) chatMsgsEl.removeChild(chatMsgsEl.firstChild);
  chatMsgsEl.scrollTop = chatMsgsEl.scrollHeight;
  if(!chatOpen){
    unreadCount++;
    chatBadge.textContent = unreadCount > 9 ? '9+' : unreadCount;
    chatBadge.style.display = 'flex';
  }
}

function addSysMsg(txt){
  var div = document.createElement('div');
  div.className = 'chat-msg cm-sys';
  div.textContent = txt;
  chatMsgsEl.appendChild(div);
  chatMsgsEl.scrollTop = chatMsgsEl.scrollHeight;
}

function escHtml(s){
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

// chat handled by SSE

function stopChatPoll(){ clearInterval(chatPollTmr); }

// Clean up old chat messages (keep last 5 min only) - called once on join
function cleanOldChat(){
  var cutoff = Date.now() - 5*60*1000;
  fetch(FB+'/apexdrift/chat.json')
    .then(function(r){ return r.json(); })
    .then(function(data){
      if(!data) return;
      Object.keys(data).forEach(function(key){
        var m = data[key];
        if(m && m.ts && m.ts < cutoff){
          fetch(FB+'/apexdrift/chat/'+key+'.json',{method:'DELETE'}).catch(function(){});
        }
      });
    }).catch(function(){});
}

// =============================================
// 5-MINUTE KICK: write joinedAt, check on interval
// =============================================
var SESSION_LIMIT_MS = 5 * 60 * 1000; // 5 minutes
var sessionTimer = null;
var joinedAt = 0;
var sessionWarned = false;

function startSessionTimer(){
  joinedAt = Date.now();
  sessionTimer = setInterval(function(){
    var elapsed = Date.now() - joinedAt;
    var remaining = SESSION_LIMIT_MS - elapsed;
    if(remaining <= 60000 && !sessionWarned){
      sessionWarned = true;
      if(isMP) addSysMsg('1 minute remaining in session!');
    }
    if(remaining <= 0){
      clearInterval(sessionTimer);
      kickPlayer();
    }
  }, 5000);
}

function kickPlayer(){
  gameOn = false; done = true;
  showKickScreen('TIMED OUT', 'Your 5-minute session has ended.\n\nRefresh to rejoin the race!');
}

// Also kick players who have been in Firebase for >5 min
// We check their 'joinedAt' field when polling
function checkPlayerExpiry(data){
  var now = Date.now();
  Object.keys(data).forEach(function(id){
    var p = data[id];
    if(!p || id === myId) return;
    if(p.joinedAt && (now - p.joinedAt) > SESSION_LIMIT_MS + 30000){
      // They've been here >5.5min and haven't cleaned up - remove them
      fetch(FB+'/apexdrift/players/'+id+'.json',{method:'DELETE'}).catch(function(){});
    }
  });
}


// =============================================
// AUDIO ENGINE - Web Audio API
// =============================================
var audioCtx = null;
var engineOsc = null, engineGain = null, engineOsc2 = null;
var driftNoise = null, driftGain = null;
var turboOsc = null, turboGain = null;
var lastGearSound = 0;
var audioStarted = false;

function initAudio(){
  if(audioStarted) return;
  audioStarted = true;
  try{
    audioCtx = new (window.AudioContext || window.webkitAudioContext)();

    // -- MASTER GAIN --
    var master = audioCtx.createGain();
    master.gain.value = 0.6;
    master.connect(audioCtx.destination);

    // -- ENGINE OSCILLATOR (main tone) --
    engineOsc = audioCtx.createOscillator();
    engineOsc.type = engineType==='electric' ? 'sawtooth' : 'sawtooth';
    engineOsc.frequency.value = 80;
    engineGain = audioCtx.createGain();
    engineGain.gain.value = 0;

    // Distortion for petrol grit
    var dist = audioCtx.createWaveShaper();
    dist.curve = makeDistCurve(engineType==='electric' ? 60 : 200);
    dist.oversample = '4x';

    // Low pass filter - shapes the tone
    var lpf = audioCtx.createBiquadFilter();
    lpf.type = 'lowpass';
    lpf.frequency.value = engineType==='electric' ? 3000 : 1800;
    lpf.Q.value = 2.5;

    engineOsc.connect(dist);
    dist.connect(lpf);
    lpf.connect(engineGain);
    engineGain.connect(master);
    engineOsc.start();

    // -- ENGINE OSC 2 (harmonic layer) --
    engineOsc2 = audioCtx.createOscillator();
    engineOsc2.type = engineType==='electric' ? 'sine' : 'triangle';
    engineOsc2.frequency.value = 160;
    var eg2 = audioCtx.createGain();
    eg2.gain.value = 0;
    engineGain.connect = engineGain.connect; // keep ref
    var lpf2 = audioCtx.createBiquadFilter();
    lpf2.type = 'lowpass';
    lpf2.frequency.value = 2400;
    engineOsc2.connect(lpf2);
    lpf2.connect(eg2);
    eg2.connect(master);
    engineOsc2.start();
    engineGain._osc2gain = eg2;

    // -- DRIFT / TIRE SQUEAL (white noise) --
    var noiseBuffer = audioCtx.createBuffer(1, audioCtx.sampleRate*0.5, audioCtx.sampleRate);
    var nd = noiseBuffer.getChannelData(0);
    for(var i=0;i<nd.length;i++) nd[i]=(Math.random()*2-1);
    driftNoise = audioCtx.createBufferSource();
    driftNoise.buffer = noiseBuffer;
    driftNoise.loop = true;
    driftGain = audioCtx.createGain();
    driftGain.gain.value = 0;
    var bpf = audioCtx.createBiquadFilter();
    bpf.type = 'bandpass';
    bpf.frequency.value = 1200;
    bpf.Q.value = 0.8;
    driftNoise.connect(bpf);
    bpf.connect(driftGain);
    driftGain.connect(master);
    driftNoise.start();

    // -- TURBO WHINE (electric whine / petrol turbo) --
    turboOsc = audioCtx.createOscillator();
    turboOsc.type = 'sine';
    turboOsc.frequency.value = 800;
    turboGain = audioCtx.createGain();
    turboGain.gain.value = 0;
    var tDist = audioCtx.createWaveShaper();
    tDist.curve = makeDistCurve(30);
    turboOsc.connect(tDist);
    tDist.connect(turboGain);
    turboGain.connect(master);
    turboOsc.start();

  }catch(e){ console.warn('Audio init failed:', e); }
}

function makeDistCurve(amount){
  var n=256, curve=new Float32Array(n), x;
  for(var i=0;i<n;i++){
    x=i*2/n-1;
    curve[i]=((Math.PI+amount)*x)/(Math.PI+amount*Math.abs(x));
  }
  return curve;
}

function updateAudio(DT){
  if(!audioCtx||!engineOsc) return;
  var t = audioCtx.currentTime;

  // RPM -> frequency mapping
  var rpmNorm = (rpm-RPM_IDLE)/(RPM_MAX-RPM_IDLE); // 0-1
  var spd = Math.abs(carSpdMS);

  if(engineType==='electric'){
    // Electric: smooth whine, pitch follows speed directly
    var baseFreq = 60 + spd * 3.2;
    var harmFreq  = baseFreq * 2.5;
    engineOsc.frequency.setTargetAtTime(baseFreq, t, 0.04);
    if(engineGain._osc2gain) engineGain._osc2gain.gain.setTargetAtTime(fwdKey ? 0.18 : 0.05, t, 0.05);
    // Turbo whine = high pitch electric motor
    var whineFreq = 400 + spd * 8;
    turboOsc.frequency.setTargetAtTime(Math.min(whineFreq, 3200), t, 0.03);
    turboGain.gain.setTargetAtTime(fwdKey ? 0.12 + rpmNorm*0.1 : 0.03, t, 0.06);
  } else {
    // Petrol: RPM-driven rumble
    var baseFreq = 55 + rpmNorm * 140;
    var harmFreq  = baseFreq * 1.5;
    engineOsc.frequency.setTargetAtTime(baseFreq, t, 0.06);
    if(engineGain._osc2gain) engineGain._osc2gain.gain.setTargetAtTime(fwdKey ? 0.12 : 0.04, t, 0.07);
    // Turbo: whoosh at high RPM
    var tFreq = 300 + rpmNorm*600;
    turboOsc.frequency.setTargetAtTime(tFreq, t, 0.08);
    turboGain.gain.setTargetAtTime(rpmNorm > 0.7 ? (rpmNorm-0.7)*0.15 : 0, t, 0.1);
  }

  // Engine volume: louder when throttle
  var targetVol = fwdKey ? 0.25 + rpmNorm*0.28 : (spd>1 ? 0.08+rpmNorm*0.08 : 0.04);
  engineGain.gain.setTargetAtTime(targetVol, t, 0.05);

  // Drift squeal
  var isDrift = (keys[' ']||keys['Shift']||keys['ShiftLeft']||keys['ShiftRight']) && spd>3;
  var driftVol = isDrift ? Math.min(0.35, driftAmt*0.9) : (driftAmt>0.12 ? driftAmt*0.3 : 0);
  driftGain.gain.setTargetAtTime(driftVol, t, 0.08);

  // Wall hit clunk - handled in resolveWalls via playClunk()
}

function playClunk(){
  if(!audioCtx) return;
  try{
    var osc=audioCtx.createOscillator();
    var g=audioCtx.createGain();
    osc.type='square';
    osc.frequency.value=120;
    osc.frequency.setTargetAtTime(40,audioCtx.currentTime,0.05);
    g.gain.value=0.5;
    g.gain.setTargetAtTime(0,audioCtx.currentTime+0.01,0.04);
    osc.connect(g); g.connect(audioCtx.destination);
    osc.start(); osc.stop(audioCtx.currentTime+0.18);
  }catch(e){}
}

function playShiftClick(){
  if(!audioCtx) return;
  try{
    var osc=audioCtx.createOscillator();
    var g=audioCtx.createGain();
    osc.type='square';
    osc.frequency.value=engineType==='electric'?600:300;
    g.gain.value=0.15;
    g.gain.setTargetAtTime(0,audioCtx.currentTime+0.02,0.03);
    osc.connect(g); g.connect(audioCtx.destination);
    osc.start(); osc.stop(audioCtx.currentTime+0.1);
  }catch(e){}
}

function playCountdownBeep(isGo){
  if(!audioCtx) return;
  try{
    var osc=audioCtx.createOscillator();
    var g=audioCtx.createGain();
    osc.type='sine';
    osc.frequency.value=isGo?880:440;
    g.gain.value=0.3;
    g.gain.setTargetAtTime(0,audioCtx.currentTime+(isGo?0.3:0.12),0.05);
    osc.connect(g); g.connect(audioCtx.destination);
    osc.start(); osc.stop(audioCtx.currentTime+(isGo?0.4:0.15));
  }catch(e){}
}


// =============================================
// LOOP
// =============================================
var lastFT=performance.now(),mmFr=0,MAX_DT=1/30;
function loop(){
  requestAnimationFrame(loop);
  var now=performance.now();
  var DT=Math.min((now-lastFT)/1000,MAX_DT);lastFT=now;
  if(!paused){tick(DT);updateCam();updateRemotes();if(++mmFr%4===0)drawMM();}
  renderer.render(scene,camera);
}
</script>
</body>
</html>
