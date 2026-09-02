# incord-code
An autonomous coding harness. It plans work, delegates it to a coding agent you already have installed and logged in, and proves the work was done before it calls it done.


[coding_agent_blob_incord_pixel_intro.html](https://github.com/user-attachments/files/31758644/coding_agent_blob_incord_pixel_intro.html)
<h2 class="sr-only">An animated blob-shaped coding agent. On load a pixel wordmark reading Incord lights up, then hands off to a code line display with six working states: idle, thinking, writing, running, blocked and done.</h2>
<div class="stage st-idle">
<div class="aura"></div>
<div class="blob">
<div class="face"><div class="eye l"></div><div class="eye r"></div></div>
<div class="dots"><i></i><i></i><i></i></div>
</div>
<div class="pix"></div>
<div class="term hid">
<div class="sym">&gt;_</div>
<div class="lines"><div class="ln"></div><div class="ln"></div><div class="ln"></div><div class="ln"></div><div class="caret"></div></div>
</div>
<div class="status">booting</div>
</div>
<div class="ctl">
<button data-s="idle">Idle</button>
<button data-s="thinking">Thinking</button>
<button data-s="writing">Writing</button>
<button data-s="running">Running</button>
<button data-s="blocked">Blocked</button>
<button data-s="done">Done</button>
<button data-r="1">Replay intro</button>
</div>
<style>
.sr-only{position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0 0 0 0)}
.stage{position:relative;height:380px;--c1:#ffc98a;--c2:#ef7c14;--c3:#7a3600;--ac:255,158,66}
.st-thinking{--c1:#c3a8ff;--c2:#7a4ee0;--c3:#3b1f7a;--ac:170,130,255}
.st-writing{--c1:#8fe6f2;--c2:#1f92a4;--c3:#0c4a56;--ac:110,220,235}
.st-running{--c1:#ffe89a;--c2:#d9a017;--c3:#6e4d00;--ac:255,208,105}
.st-blocked{--c1:#ff9aa6;--c2:#d92a40;--c3:#6e0e1c;--ac:255,90,110}
.st-done{--c1:#a6f5a0;--c2:#2fae3a;--c3:#125218;--ac:130,240,130}
.aura{position:absolute;left:50%;top:40%;width:250px;height:250px;transform:translate(-50%,-50%);background:radial-gradient(circle at 50% 50%,rgba(var(--ac),.5) 0%,rgba(var(--ac),.16) 46%,transparent 72%);filter:blur(26px);opacity:.75;z-index:5;animation:morph 9s ease-in-out infinite,breathe 5s ease-in-out infinite alternate}
.blob{position:absolute;left:50%;top:40%;width:190px;height:190px;transform:translate(-50%,-50%);z-index:20;background:radial-gradient(circle at 33% 26%,var(--c1) 0%,var(--c2) 44%,var(--c3) 100%);box-shadow:inset -16px -20px 40px rgba(0,0,0,.42),inset 12px 14px 26px rgba(255,255,255,.28),0 14px 30px rgba(0,0,0,.2);animation:morph 9s ease-in-out infinite,bob 6s ease-in-out infinite;transition:background .4s ease}
.st-thinking .blob{animation:morph 5s ease-in-out infinite,bob 4s ease-in-out infinite}
.st-writing .blob{animation:morph 3.4s ease-in-out infinite,bob 1.6s ease-in-out infinite}
.st-running .blob{animation:morph 2.6s ease-in-out infinite,thump .7s ease-in-out infinite}
.st-blocked .blob{animation:rigid .3s ease-in-out infinite}
.st-done .blob{animation:morph 6s ease-in-out infinite,hop .9s cubic-bezier(.3,0,.4,1) infinite}
.face{position:absolute;top:38%;left:50%;display:flex;gap:22px;transform:translateX(-50%);z-index:30;will-change:transform}
.eye{width:12px;height:30px;border-radius:5px;background:#fff;animation:blink 4.4s infinite;transition:width .22s,height .22s,border-radius .22s}
.st-thinking .eye{height:20px}
.st-writing .eye{height:14px;width:14px;border-radius:4px}
.st-running .eye{height:26px;animation:blink 1.6s infinite}
.st-blocked .eye{height:22px;width:13px;animation:none}
.st-blocked .eye.l{transform:rotate(26deg)}
.st-blocked .eye.r{transform:rotate(-26deg)}
.st-done .eye{width:28px;height:15px;border-radius:28px 28px 0 0;animation:none}
.dots{position:absolute;top:19%;left:50%;transform:translateX(-50%);display:flex;gap:5px;z-index:30;opacity:0;transition:opacity .3s}
.st-thinking .dots{opacity:1}
.dots i{width:6px;height:6px;border-radius:50%;background:#fff;animation:think 1.3s ease-in-out infinite}
.dots i:nth-child(2){animation-delay:.18s}
.dots i:nth-child(3){animation-delay:.36s}
.pix{position:absolute;bottom:16%;left:50%;transform:translateX(-50%);display:grid;grid-template-columns:repeat(35,4px);grid-auto-rows:4px;gap:1px;z-index:18;transition:opacity .5s ease}
.pix span{border-radius:1px;opacity:0;transform:scale(.4);transition:opacity .18s ease,transform .18s ease}
.pix span.on{background:rgb(var(--ac))}
.pix span.lit{opacity:1;transform:scale(1)}
.term{position:absolute;bottom:14%;left:50%;transform:translateX(-50%);display:flex;flex-direction:column;align-items:center;gap:10px;z-index:15;transition:opacity .5s ease}
.hid{opacity:0}
.sym{font-family:var(--font-mono);font-size:19px;letter-spacing:2px;color:rgb(var(--ac));transition:color .4s ease}
.st-idle .sym{animation:pulse 1.1s steps(1,end) infinite}
.st-thinking .sym{animation:fade 1.6s ease-in-out infinite}
.st-running .sym{animation:scan .9s linear infinite}
.st-done .sym{animation:pop 1s ease-out infinite}
.lines{display:flex;flex-wrap:wrap;align-items:center;justify-content:center;width:210px;gap:5px}
.ln{height:4px;border-radius:2px;background:rgba(var(--ac),.8);width:20%;transition:background .4s}
.st-writing .ln{animation:type 2.4s ease-in-out infinite}
.st-writing .ln:nth-child(2){animation-delay:.3s}
.st-writing .ln:nth-child(3){animation-delay:.6s}
.st-writing .ln:nth-child(4){animation-delay:.9s}
.st-running .ln{animation:scan 1.1s linear infinite}
.st-running .ln:nth-child(2){animation-delay:.15s}
.st-running .ln:nth-child(3){animation-delay:.3s}
.st-running .ln:nth-child(4){animation-delay:.45s}
.st-blocked .ln{opacity:.25}
.caret{width:8px;height:14px;border-radius:2px;background:rgb(var(--ac));animation:pulse 1s steps(1,end) infinite}
.st-blocked .caret{opacity:.2;animation:none}
.status{position:absolute;bottom:3%;left:0;right:0;text-align:center;font-family:var(--font-mono);font-size:13px;color:var(--text-secondary)}
.ctl{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin-top:12px}
@keyframes morph{0%,100%{border-radius:58% 42% 46% 54%/48% 52% 48% 52%}25%{border-radius:44% 56% 62% 38%/56% 44% 56% 44%}50%{border-radius:52% 48% 38% 62%/42% 58% 42% 58%}75%{border-radius:62% 38% 54% 46%/58% 42% 58% 42%}}
@keyframes rigid{0%,100%{border-radius:22%;transform:translate(-50%,-50%) translateX(-3px)}50%{border-radius:22%;transform:translate(-50%,-50%) translateX(3px)}}
@keyframes bob{0%,100%{transform:translate(-50%,-50%)}50%{transform:translate(-50%,calc(-50% - 10px))}}
@keyframes thump{0%,100%{transform:translate(-50%,-50%) scale(1)}50%{transform:translate(-50%,-50%) scale(1.06)}}
@keyframes hop{0%{transform:translate(-50%,calc(-50% + 8px)) scale(1.06,.94)}40%{transform:translate(-50%,calc(-50% - 18px)) scale(.95,1.06)}100%{transform:translate(-50%,calc(-50% + 8px)) scale(1.06,.94)}}
@keyframes breathe{0%{opacity:.5;transform:translate(-50%,-50%) scale(.94)}100%{opacity:.85;transform:translate(-50%,-50%) scale(1.08)}}
@keyframes blink{0%,95%,100%{height:30px}97%{height:3px}}
@keyframes think{0%,100%{transform:translateY(0);opacity:.4}50%{transform:translateY(-6px);opacity:1}}
@keyframes type{0%{width:6%}45%{width:44%}55%{width:44%}100%{width:6%}}
@keyframes scan{0%,100%{opacity:.3}50%{opacity:1}}
@keyframes fade{0%,100%{opacity:.45}50%{opacity:1}}
@keyframes pop{0%{transform:scale(1)}10%{transform:scale(1.22)}26%{transform:scale(1)}100%{transform:scale(1)}}
@keyframes pulse{0%,49%{opacity:1}50%,100%{opacity:0}}
</style>
<script>
(function(){
var stage=document.querySelector('.stage'),blob=document.querySelector('.blob'),face=document.querySelector('.face'),status=document.querySelector('.status'),sym=document.querySelector('.sym'),pix=document.querySelector('.pix'),term=document.querySelector('.term');
var F={I:['11111','00100','00100','00100','00100','00100','11111'],N:['10001','11001','11001','10101','10011','10011','10001'],C:['01110','10001','10000','10000','10000','10001','01110'],O:['01110','10001','10001','10001','10001','10001','01110'],R:['11110','10001','10001','11110','10100','10010','10001'],D:['11110','10001','10001','10001','10001','10001','11110']};
var W='INCORD',cells=[];
for(var r=0;r<7;r++){for(var w=0;w<W.length;w++){var g=F[W[w]][r];for(var c=0;c<5;c++){var s=document.createElement('span');if(g[c]==='1')s.className='on';s.dataset.col=w*6+c;pix.appendChild(s);cells.push(s);}if(w<W.length-1){var b=document.createElement('span');b.dataset.col=w*6+5;pix.appendChild(b);cells.push(b);}}}
var timers=[];
function intro(){
timers.forEach(clearTimeout);timers=[];
pix.style.opacity='1';term.classList.add('hid');status.textContent='booting';
cells.forEach(function(s){s.classList.remove('lit');});
cells.forEach(function(s){if(!s.classList.contains('on'))return;
timers.push(setTimeout(function(){s.classList.add('lit');},parseInt(s.dataset.col,10)*26+Math.random()*90));});
timers.push(setTimeout(function(){status.textContent='incord · agent online';},1250));
timers.push(setTimeout(function(){pix.style.opacity='0';},2600));
timers.push(setTimeout(function(){term.classList.remove('hid');status.textContent=label[st];},3050));
}
var st='idle',nx=0,ny=0,ex=0,ey=0,last=-9999;
var cfg={idle:{ax:9,ay:8,ease:.10,bx:0,by:0,track:1},thinking:{ax:5,ay:4,ease:.05,bx:-7,by:-7,track:0},writing:{ax:4,ay:3,ease:.12,bx:0,by:7,track:0},running:{ax:7,ay:6,ease:.16,bx:0,by:0,track:1},blocked:{ax:11,ay:8,ease:.30,bx:0,by:0,track:1},done:{ax:6,ay:5,ease:.10,bx:0,by:0,track:1}};
var mark={idle:'>_',thinking:'/* … */',writing:'</>',running:'$ ./',blocked:'[ ! ]',done:'<>'};
var label={idle:'idle · waiting for a task',thinking:'planning · reading context',writing:'writing · src/brief.rs',running:'running · cargo test',blocked:'blocked · guard rule 1',done:'done · 4 tests passed'};
function aim(e){var s=stage.getBoundingClientRect(),r=blob.getBoundingClientRect();
var dx=(e.clientX-(r.left+r.width/2))/(s.width*.4),dy=(e.clientY-(r.top+r.height/2))/(s.height*.4);
var L=Math.sqrt(dx*dx+dy*dy);if(L>1){dx/=L;dy/=L;}nx=dx;ny=dy;last=performance.now();}
document.addEventListener('pointermove',aim);
function loop(t){var c=cfg[st],ux=nx,uy=ny;
if(!c.track){ux=0;uy=0;}else if(t-last>2400){var s=t/1600;ux=Math.sin(s)*.5;uy=Math.cos(s*.63)*.4;}
ex+=(ux*c.ax+c.bx-ex)*c.ease;ey+=(uy*c.ay+c.by-ey)*c.ease;
face.style.transform='translate(calc(-50% + '+ex.toFixed(2)+'px),'+ey.toFixed(2)+'px)';
requestAnimationFrame(loop);}
requestAnimationFrame(loop);
Array.prototype.forEach.call(document.querySelectorAll('.ctl button'),function(b){b.addEventListener('click',function(){
if(b.dataset.r){intro();return;}
st=b.dataset.s;stage.className='stage st-'+st;status.textContent=label[st];sym.textContent=mark[st];});});
intro();
})();
</script>
