
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Đánh Quái Qua Màn</title>
<style>
*{box-sizing:border-box}
body{
  margin:0;background:#111827;color:white;
  font-family:Arial,sans-serif;text-align:center
}
h2{margin:12px}
#info{display:flex;justify-content:center;gap:16px;flex-wrap:wrap}
canvas{
  background:linear-gradient(#243b55,#141e30);
  width:min(95vw,800px);height:auto;
  border:3px solid #64748b;border-radius:10px;
  touch-action:none
}
button{
  background:#2563eb;color:white;border:0;
  border-radius:10px;padding:13px 18px;
  font-size:16px;margin:5px;cursor:pointer
}
button:active{background:#1d4ed8}
#controls{max-width:600px;margin:auto}
</style>
</head>
<body>
<h2>⚔️ ĐÁNH QUÁI QUA MÀN</h2>
<div id="info">
  <p id="hp">❤️ Máu: 100</p>
  <p id="stage">🌍 Màn: 1</p>
  <p id="kills">👹 Quái: 0</p>
</div>
<canvas id="game" width="800" height="440"></canvas>
<div id="controls">
  <div>
    <button id="left">⬅️ Trái</button>
    <button id="right">➡️ Phải</button>
    <button id="jump">⬆️ Nhảy</button>
    <button id="attack">⚔️ Đánh</button>
  </div>
  <button id="restart">🔄 Chơi lại</button>
</div>
<p>Máy tính: A/D hoặc ←/→ để di chuyển, Space để nhảy, J để đánh.</p>
<script>
const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");
const W=canvas.width,H=canvas.height;
let keys={},player,monster,stage,kills,gameOver,won;
let attackTimer=0,spawnTimer=0,invincible=0;

function reset(){
  player={x:100,y:330,w:34,h:50,hp:100,
    vx:0,vy:0,face:1,onGround:true};
  stage=1;kills=0;gameOver=false;won=false;
  attackTimer=0;spawnTimer=0;invincible=0;
  spawnMonster();updateInfo();
}
function spawnMonster(){
  let boss=stage%5===0;
  monster={
    x:Math.random()<.5?560:700,y:340,w:boss?65:42,h:boss?70:48,
    hp:boss?100+stage*15:35+stage*12,
    maxHp:boss?100+stage*15:35+stage*12,
    damage:boss?18:7+stage,
    speed:boss?0.65:0.7+stage*.12,
    boss
  };
}
function updateInfo(){
  document.getElementById("hp").textContent=
    "❤️ Máu: "+Math.max(0,Math.ceil(player.hp));
  document.getElementById("stage").textContent="🌍 Màn: "+stage;
  document.getElementById("kills").textContent="👹 Quái: "+kills;
}
function attack(){
  if(gameOver||won||attackTimer>0)return;
  attackTimer=18;
  if(Math.abs((player.x+player.w/2)-(monster.x+monster.w/2))<95){
    monster.hp-=20+stage*2;
    if(monster.hp<=0){
      kills++;
      if(stage>=10){won=true}
      else stage++;
      if(!won)spawnMonster();
    }
  }
}
function update(){
  if(gameOver||won)return;
  player.vx=0;
  if(keys.ArrowLeft||keys.a){player.vx=-3.5;player.face=-1}
  if(keys.ArrowRight||keys.d){player.vx=3.5;player.face=1}
  player.x=Math.max(0,Math.min(W-player.w,player.x+player.vx));
  player.vy+=0.55;player.y+=player.vy;
  if(player.y+player.h>=380){
    player.y=380-player.h;player.vy=0;player.onGround=true;
  }
  if(attackTimer>0)attackTimer--;
  if(invincible>0)invincible--;
  let dx=(player.x+player.w/2)-(monster.x+monster.w/2);
  if(Math.abs(dx)>45)monster.x+=Math.sign(dx)*monster.speed;
  if(Math.abs(dx)<monster.w/2+player.w/2+4&&invincible===0){
    player.hp-=monster.damage;
    invincible=45;
    if(player.hp<=0){player.hp=0;gameOver=true}
  }
  updateInfo();
}
function bar(x,y,w,h,pct,color){
  ctx.fillStyle="#374151";ctx.fillRect(x,y,w,h);
  ctx.fillStyle=color;ctx.fillRect(x,y,w*Math.max(0,pct),h);
}
function draw(){
  ctx.clearRect(0,0,W,H);
  // Mặt đất
  ctx.fillStyle="#166534";ctx.fillRect(0,380,W,60);
  ctx.fillStyle="#4ade80";ctx.fillRect(0,380,W,5);
  // Trang trí
  ctx.fillStyle="#334155";
  for(let i=0;i<8;i++)ctx.fillRect(i*115+20,310,45,70);
  // Người chơi
  if(invincible%8<4){
    ctx.fillStyle="#60a5fa";
    ctx.fillRect(player.x,player.y,player.w,player.h);
  }
  ctx.fillStyle="white";ctx.font="15px Arial";
  ctx.fillText("HERO",player.x-3,player.y-8);
  // Kiếm
  if(attackTimer>0){
    ctx.strokeStyle="#facc15";ctx.lineWidth=7;
    ctx.beginPath();
    let sx=player.x+player.w/2;
    ctx.moveTo(sx,player.y+20);
    ctx.lineTo(sx+player.face*55,player.y+10);
    ctx.stroke();
  }
  // Quái
  ctx.fillStyle=monster.boss?"#a855f7":"#ef4444";
  ctx.fillRect(monster.x,monster.y,monster.w,monster.h);
  ctx.fillStyle="white";
  ctx.fillRect(monster.x+8,monster.y+10,7,7);
  ctx.fillRect(monster.x+monster.w-15,monster.y+10,7,7);
  bar(monster.x,monster.y-17,monster.w,9,
      monster.hp/monster.maxHp,"#f87171");
  ctx.fillStyle="white";ctx.font="14px Arial";
  ctx.fillText(monster.boss?"BOSS":"QUÁI",
      monster.x,monster.y-23);
  ctx.fillText("MÀN "+stage,20,30);
  if(gameOver||won){
    ctx.fillStyle="rgba(0,0,0,.75)";
    ctx.fillRect(0,0,W,H);
    ctx.textAlign="center";
    ctx.fillStyle=won?"#facc15":"#f87171";
    ctx.font="bold 42px Arial";
    ctx.fillText(won?"CHIẾN THẮNG!":"BẠN ĐÃ THUA!",W/2,190);
    ctx.fillStyle="white";ctx.font="22px Arial";
    ctx.fillText(won?"Bạn đã vượt qua 10 màn!":"Hãy thử lại nhé!",W/2,235);
    ctx.textAlign="left";
  }
}
function loop(){update();draw();requestAnimationFrame(loop)}
document.addEventListener("keydown",e=>{
  keys[e.key]=true;
  if(["ArrowLeft","ArrowRight"," "].includes(e.key))e.preventDefault();
  if(e.key===" "&&player.onGround&&!gameOver&&!won){
    player.vy=-11;player.onGround=false;
  }
  if(e.key.toLowerCase()==="j")attack();
});
document.addEventListener("keyup",e=>keys[e.key]=false);
function hold(id,key){
  let b=document.getElementById(id);
  b.addEventListener("pointerdown",e=>{
    e.preventDefault();keys[key]=true;
  });
  for(let ev of ["pointerup","pointercancel","pointerleave"])
    b.addEventListener(ev,()=>keys[key]=false);
}
hold("left","ArrowLeft");hold("right","ArrowRight");
document.getElementById("jump").addEventListener("click",()=>{
  if(player.onGround&&!gameOver&&!won){
    player.vy=-11;player.onGround=false;
  }
});
document.getElementById("attack").addEventListener("click",attack);
document.getElementById("restart").addEventListener("click",reset);
reset();loop();
</script>
</body>
</html>
