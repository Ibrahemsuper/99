<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Interactive Academy Background</title>
<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
  background: radial-gradient(circle at center, #0a0742, #02000f);
}
canvas {
  display: block;
}
</style>
</head>
<body>

<canvas id="bgCanvas"></canvas>

<script>
const canvas = document.getElementById("bgCanvas");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let dots = [];
let mouse = { x: null, y: null };

window.onmousemove = function(e) {
  mouse.x = e.clientX;
  mouse.y = e.clientY;
};

class Dot {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = 2 + Math.random()*2;
    this.baseX = x;
    this.baseY = y;
  }
  draw() {
    ctx.fillStyle = "rgba(0,255,255,0.7)";
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size, 0, Math.PI*2);
    ctx.fill();
  }
  update() {
    let dx = mouse.x - this.x;
    let dy = mouse.y - this.y;
    let dist = Math.sqrt(dx*dx + dy*dy);
    let effect = (dist < 150) ? (150 - dist)/150 : 0;
    this.x -= dx * 0.015 * effect;
    this.y -= dy * 0.015 * effect;

    this.x += (this.baseX - this.x)*0.08;
    this.y += (this.baseY - this.y)*0.08;
  }
}

function init() {
  dots = [];
  for(let y=0; y<canvas.height; y+=20) {
    for(let x=0; x<canvas.width; x+=20) {
      dots.push(new Dot(x, y));
    }
  }
}

function connectDots() {
  for (let i = 0; i < dots.length; i++) {
    for (let j = i; j < dots.length; j++) {
      let dx = dots[i].x - dots[j].x;
      let dy = dots[i].y - dots[j].y;
      let dist = dx*dx + dy*dy;
      if (dist < 400) {
        ctx.strokeStyle = "rgba(0,255,255,0.15)";
        ctx.beginPath();
        ctx.moveTo(dots[i].x, dots[i].y);
        ctx.lineTo(dots[j].x, dots[j].y);
        ctx.stroke();
      }
    }
  }
}

function animate() {
  ctx.clearRect(0,0,canvas.width,canvas.height);
  dots.forEach(dot => {
    dot.update();
    dot.draw();
  });
  connectDots();
  requestAnimationFrame(animate);
}

init();
animate();
window.onresize = init;
</script>

</body>
</html>
