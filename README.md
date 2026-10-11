<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>CaFVF LDO: two loops, live</title>
<style>
:root{--bg:#f4f7f9;--ink:#15212b;--mute:#586877;--rule:#cbd5dd;--panel:#e8eef3;--il:#c46a14;--ip:#0b6fa8;--vo:#1d8a5a;--in:#0b6fa8;--out:#c2403a;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0f161c;--ink:#e2e9ef;--mute:#92a2b0;--rule:#2a3743;--panel:#17212a;--il:#f0a050;--ip:#5db5ea;--vo:#5fd09a;--in:#5db5ea;--out:#ef7d76}}
:root[data-theme="dark"]{--bg:#0f161c;--ink:#e2e9ef;--mute:#92a2b0;--rule:#2a3743;--panel:#17212a;--il:#f0a050;--ip:#5db5ea;--vo:#5fd09a;--in:#5db5ea;--out:#ef7d76}
html{height:auto}*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font:15px/1.5 system-ui,-apple-system,"Segoe UI",sans-serif}
header{padding:1rem 1.2rem .4rem;max-width:80rem;margin:0 auto}
h1{font-size:1.4rem;margin:0}header p{margin:.2rem 0;color:var(--mute);max-width:60rem}
.app{display:grid;gap:1.2rem;padding:.8rem 1.2rem 2rem;max-width:80rem;margin:0 auto}
@media(min-width:920px){.app{grid-template-columns:320px 1fr}}
aside,.chart{background:var(--panel);border-radius:6px;padding:.8rem}
aside h2{font-size:1rem;margin:.9rem 0 .3rem}aside h2:first-child{margin-top:0}
.row{display:grid;grid-template-columns:1fr auto;font-size:.88rem;margin-top:.35rem}.row output{font-variant-numeric:tabular-nums;color:var(--mute)}
.row input[type=range]{grid-column:1/3;width:100%}
select,button{font:inherit;padding:.3rem .6rem;border:1px solid var(--rule);background:var(--bg);color:var(--ink);border-radius:4px}
button{cursor:pointer}button:focus-visible,select:focus-visible,input:focus-visible{outline:2px solid var(--in);outline-offset:2px}
.bar{display:flex;gap:.6rem;flex-wrap:wrap;align-items:center;margin-bottom:.8rem}
.chart{margin-bottom:.8rem}.chart h3{font-size:.88rem;margin:0 0 .3rem;display:flex;justify-content:space-between;flex-wrap:wrap;gap:.4rem}
.chart h3 span{font-weight:400;color:var(--mute)}
canvas{width:100%;height:130px;display:block;cursor:crosshair}
table{width:100%;border-collapse:collapse;font-size:.85rem}td{padding:.2rem 0;border-bottom:1px solid var(--rule)}td:last-child{text-align:right;font-variant-numeric:tabular-nums}
svg text{fill:var(--ink);font-size:11px}svg rect{fill:var(--bg);stroke:var(--mute)}
.lp{fill:none;stroke-linejoin:round}#lin{stroke:var(--in)}#lout{stroke:var(--out)}
.k{display:inline-block;width:.8rem;height:.3rem;margin-right:.3rem;vertical-align:middle}
.note{color:var(--mute);font-size:.8rem;margin-top:.8rem}
</style></head><body>
<header><h1>CaFVF LDO: both loops, live</h1>
<p>Behavioral model of the double-buffered CaFVF LDO (Albezzano et al., PRIME 2025). Apply a load step and watch the fast inner loop catch it, then the slow OTA loop clean up the error. Press Play, or click any chart to place the cursor.</p></header>
<div class="app">
<aside>
<h2>Load</h2><div id="c-load"></div>
<div class="row"><label for="prof">Profile</label><select id="prof"><option value="step">Step up</option><option value="pulse">Pulse (30 ns)</option></select></div>
<h2>Design</h2><div id="c-des"></div>
<div class="row"><label for="buf">Double buffer (SSF)</label><input type="checkbox" id="buf" checked></div>
<h2>Loops</h2>
<svg viewBox="0 0 310 215" role="img" aria-label="Block diagram of inner and outer loops" style="width:100%">
<rect x="190" y="15" width="90" height="34"/><text x="235" y="36" text-anchor="middle">Mp (pass)</text>
<rect x="190" y="125" width="90" height="34"/><text x="235" y="146" text-anchor="middle">Mf (sense)</text>
<rect x="80" y="70" width="80" height="34"/><text x="120" y="91" text-anchor="middle">Mc (cascode)</text>
<rect x="80" y="15" width="80" height="34"/><text x="120" y="36" text-anchor="middle">Buffer</text>
<rect x="80" y="170" width="80" height="34"/><text x="120" y="191" text-anchor="middle">OTA</text>
<line x1="190" y1="110" x2="296" y2="110" stroke="var(--mute)" stroke-width="2"/><text x="256" y="104">Vout</text>
<text x="254" y="208">Il, CL</text>
<path id="lin" class="lp" d="M235,49 L235,125 M190,142 L120,142 L120,104 M120,70 L120,49 M160,32 L190,32"/>
<path id="lout" class="lp" d="M296,110 L296,187 L160,187 M80,187 L40,187 L40,87 L80,87"/>
<text x="6" y="12" style="fill:var(--in)">Fast inner loop</text><text x="6" y="26" style="fill:var(--out)">Slow outer loop</text>
</svg>
<h2>At cursor</h2>
<table><tr><td>Time</td><td id="r-t"></td></tr><tr><td>Load current Il</td><td id="r-il"></td></tr><tr><td>Mp current Ip</td><td id="r-ip"></td></tr><tr><td>Vout</td><td id="r-vo"></td></tr><tr><td>Gate drive Vov,p</td><td id="r-x"></td></tr><tr><td>OTA correction</td><td id="r-vc"></td></tr></table>
<h2>Results</h2>
<table id="m"></table>
<p class="note">A behavioral model, not a SPICE run. Loop-gain scale and parasitic poles are calibrated so the default design lands near the paper (about 1.7 GHz at 5 mA, 14 mV dip, 254 ps). Use it for trends. Final numbers come from the transistor-level design.</p>
</aside>
<main>
<div class="bar"><button id="play">Play</button><label>Window <select id="win"><option value="2">2 ns (fast loop)</option><option value="10" selected>10 ns</option><option value="60">60 ns (slow loop)</option></select></label></div>
<div class="chart"><h3>Load current and pass-device current <span><i class="k" style="background:var(--il)"></i>Il <i class="k" style="background:var(--ip)"></i>Ip (mA)</span></h3><canvas id="c0"></canvas></div>
<div class="chart"><h3>Output voltage <span><i class="k" style="background:var(--vo)"></i>Vout (mV), dashed: nominal and ±1% band</span></h3><canvas id="c1"></canvas></div>
<div class="chart"><h3>Fast inner loop <span><i class="k" style="background:var(--in)"></i>Mp gate overdrive Vov,p (mV)</span></h3><canvas id="c2"></canvas></div>
<div class="chart"><h3>Slow outer loop <span><i class="k" style="background:var(--out)"></i>OTA correction of the gate bias (mV)</span></h3><canvas id="c3"></canvas></div>
</main></div>
<script>
const $=id=>document.getElementById(id),K=0.2625,KC=0.105,N=60000,DT=1e-12,T0=0.5e-9;
const P={dI:10,te:0.1,CL:100,If:350,al:0.3,Rs:100,gbw:30,prof:'step',win:10,buf:true};
const SL=[['c-load','dI','Load step',1,10,1,'mA'],['c-load','te','Edge time',0.05,5,0.05,'ns'],['c-des','CL','CL',20,200,5,'pF'],['c-des','If','If',150,600,10,'µA'],['c-des','al','α (Ig,2 = α·If)',0.1,0.9,0.05,''],['c-des','Rs','Buffer Rssf',50,20000,50,'Ω'],['c-des','gbw','OTA GBW',5,100,1,'MHz']];
SL.forEach(([host,k,l,mn,mx,st,u])=>{const d=document.createElement('div');d.className='row';d.innerHTML=`<label for="s${k}">${l}</label><output id="o${k}"></output><input type="range" id="s${k}" min="${mn}" max="${mx}" step="${st}" value="${P[k]}">`;$(host).appendChild(d);const f=()=>{P[k]=+$('s'+k).value;$('o'+k).textContent=P[k]+' '+u;go()};$('s'+k).oninput=f;$('o'+k).textContent=P[k]+' '+u});
$('prof').onchange=e=>{P.prof=e.target.value;go()};$('buf').onchange=e=>{P.buf=e.target.checked;go()};
$('win').onchange=e=>{P.win=+e.target.value;ct=Math.min(ct,P.win);draw()};
let D=null,ct=0,playing=false,tick=0;
const cl=(a,b,v)=>Math.min(b,Math.max(a,v));
function pm(fu0,poles){let fc=0;for(let k=0;k<=1500;k++){const f=1e6*Math.pow(10,k*5/1500);let m=fu0/f;poles.forEach(p=>m/=Math.sqrt(1+(f/p)**2));if(m<1){fc=f;break}}if(!fc)return[0,0];let ph=-90;poles.forEach(p=>ph-=Math.atan(fc/p)*57.2958);return[fc,180+ph]}
function go(){
const If=P.If*1e-6,al=P.al,Ib=(1+al)*If,r0=1/(0.25*al*If),A=(If/0.1)*r0*KC;
const f2=2.2e9*(al*If)/105e-6,pU=[f2,8e9],pB=[Math.min(f2*r0/P.Rs,2e11),6e9,8e9],poles=P.buf?pB:pU;
const c=poles.map(f=>1-Math.exp(-2*Math.PI*f*DT)),x0=Math.sqrt(Ib/K),wo=2*Math.PI*P.gbw*1e6,CL=P.CL*1e-12,dI=P.dI*1e-3,te=P.te*1e-9,pulse=P.prof==='pulse';
const w=poles.map(()=>x0),IL=new Float32Array(N),IP=new Float32Array(N),VO=new Float32Array(N),X=new Float32Array(N),VC=new Float32Array(N);let e=0,vc=0;
for(let i=0;i<N;i++){const t=i*DT;let r=cl(0,1,(t-T0)/te);if(pulse)r-=cl(0,1,(t-T0-30e-9)/te);const Il=dI*r;
let y=x0+vc-A*e;for(let j=0;j<w.length;j++){w[j]+=c[j]*(y-w[j]);y=w[j]}
const x=Math.max(y,0),Ip=K*x*x;IL[i]=Il*1e3;IP[i]=Ip*1e3;VO[i]=900+e*1e3;X[i]=x*1e3;VC[i]=vc*1e3;
e=cl(-0.9,0.3,e+DT*(Ip-Il-Ib)/CL);vc+=DT*(-wo*A*e)}
D={IL,IP,VO,X,VC};
let mi=0,lim=25000;for(let i=0;i<lim;i++)if(VO[i]<VO[mi])mi=i;
const ref=pulse?30000:N-1,fin=VO[ref];let ts=0;for(let i=ref;i>0;i--)if(Math.abs(VO[i]-fin)>9){ts=i;break}
const eta=100*10e-3/(10e-3+Ib+100e-6),gmp=2*Math.sqrt(K*(dI+Ib)),fu0=A*gmp/(2*Math.PI*CL),b1=pm(fu0,pB),b0=pm(fu0,pU);
let pk=0;for(let i=0;i<lim;i++)pk=Math.max(pk,IP[i]);
const rows=[['Peak Vout dip',(900-VO[mi]).toFixed(1)+' mV'],['Response time',Math.max(0,(mi*DT-T0)*1e12).toFixed(0)+' ps'],['Settling (1%)',Math.max(0,(ts*DT-T0)*1e9).toFixed(1)+' ns'],['Efficiency ηI (buffers 100 µA)',eta.toFixed(1)+' %'],['Loop gain gmp·r0g,2',(gmp*r0).toFixed(0)],['Inner loop, with buffer',(b1[0]/1e9).toFixed(2)+' GHz, PM '+b1[1].toFixed(0)+'°'],['Inner loop, no buffer',(b0[0]/1e9).toFixed(2)+' GHz, PM '+b0[1].toFixed(0)+'°']];
$('m').innerHTML=rows.map(r=>`<tr><td>${r[0]}</td><td>${r[1]}</td></tr>`).join('');
let mx=1e-9,mv=1e-9;for(let i=10;i<N-1000;i+=10){mx=Math.max(mx,Math.abs(X[i+10]-X[i-10]));mv=Math.max(mv,Math.abs(VC[i+1000]-VC[i]))}D.mx=mx;D.mv=mv;draw()}
const S=[['c0',[['IL','--il'],['IP','--ip']],0],['c1',[['VO','--vo']],1],['c2',[['X','--in']],0],['c3',[['VC','--out']],0]];
function draw(){if(!D)return;const st=getComputedStyle(document.documentElement),col=n=>st.getPropertyValue(n).trim(),n=Math.round(P.win*1e-9/DT),step=Math.max(1,Math.floor(n/1000));
S.forEach(([id,ser,vo])=>{const cv=$(id),dpr=devicePixelRatio||1,W=cv.clientWidth,H=cv.clientHeight;cv.width=W*dpr;cv.height=H*dpr;const g=cv.getContext('2d');g.scale(dpr,dpr);
let lo=1e9,hi=-1e9;ser.forEach(([k])=>{for(let i=0;i<n;i+=step){lo=Math.min(lo,D[k][i]);hi=Math.max(hi,D[k][i])}});
if(vo){lo=Math.min(lo,891);hi=Math.max(hi,909)}if(hi-lo<1e-6){hi+=1;lo-=1}const pd=(hi-lo)*.1;lo-=pd;hi+=pd;
const L=44,R=6,T=6,B=16,X=i=>L+(W-L-R)*i/n,Y=v=>T+(H-T-B)*(1-(v-lo)/(hi-lo));
g.font='11px system-ui';g.fillStyle=col('--mute');g.strokeStyle=col('--rule');g.lineWidth=1;
g.fillText(hi.toFixed(hi>100?0:1),2,T+8);g.fillText(lo.toFixed(lo>100?0:1),2,H-B);
for(let q=0;q<=4;q++){const x=L+(W-L-R)*q/4;g.beginPath();g.moveTo(x,T);g.lineTo(x,H-B);g.stroke();g.fillText((P.win*q/4).toFixed(P.win<5?1:0)+(q==4?' ns':''),x-(q==4?22:0),H-3)}
if(vo){g.setLineDash([4,3]);g.strokeStyle=col('--mute');[900,891,909].forEach(v=>{g.beginPath();g.moveTo(L,Y(v));g.lineTo(W-R,Y(v));g.stroke()});g.setLineDash([])}
ser.forEach(([k,c])=>{g.strokeStyle=col(c);g.lineWidth=1.8;g.beginPath();for(let i=0;i<n;i+=step){const x=X(i),y=Y(D[k][i]);i?g.lineTo(x,y):g.moveTo(x,y)}g.stroke()});
const cx=X(Math.round(ct*1e-9/DT));g.strokeStyle=col('--ink');g.lineWidth=1;g.beginPath();g.moveTo(cx,T);g.lineTo(cx,H-B);g.stroke()});
const i=cl(10,N-1001,Math.round(ct*1e-9/DT)),f=(a,d)=>a.toFixed(d);
$('r-t').textContent=f(ct,2)+' ns';$('r-il').textContent=f(D.IL[i],2)+' mA';$('r-ip').textContent=f(D.IP[i],2)+' mA';$('r-vo').textContent=f(D.VO[i],1)+' mV';$('r-x').textContent=f(D.X[i],1)+' mV';$('r-vc').textContent=f(D.VC[i],1)+' mV';
const a=Math.min(1,Math.abs(D.X[i+10]-D.X[i-10])/(.3*D.mx)),b=Math.min(1,Math.abs(D.VC[i+1000]-D.VC[i])/(.3*D.mv));
$('lin').style.strokeWidth=1.5+5*a;$('lin').style.opacity=.35+.65*a;$('lout').style.strokeWidth=1.5+5*b;$('lout').style.opacity=.35+.65*b}
function setC(ev){const cv=ev.currentTarget,r=cv.getBoundingClientRect(),x=(ev.clientX-r.left-44)/(r.width-50);ct=cl(0,P.win,x*P.win);draw()}
S.forEach(([id])=>{$(id).addEventListener('click',setC)});
$('play').onclick=()=>{if(playing){playing=false;$('play').textContent='Play';return}playing=true;$('play').textContent='Pause';if(ct>=P.win-.01)ct=0;let last=performance.now();
const f=now=>{if(!playing)return;ct+=(now-last)/5000*P.win;last=now;if(ct>=P.win){ct=P.win;playing=false;$('play').textContent='Play'}draw();if(playing)requestAnimationFrame(f)};requestAnimationFrame(f)};
addEventListener('resize',draw);go();
</script></body></html>
