<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>Academy Background</title>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      overflow: hidden;
      font-family: Arial, sans-serif;
      background: #0f172a;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
    }

    .content {
      position: relative;
      z-index: 2;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      text-align: center;
    }

    h1 {
      font-size: 3rem;
      font-weight: 300;
      letter-spacing: 2px;
    }
  </style>
</head>

<body>

<canvas id="background"></canvas>

<div class="content">
  <h1>Welcome To Our Academy</h1>
</div>

<script>
  const canvas = document.getElementById("background");
  const ctx = canvas.getContext("2d");

  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  let particles = [];
  let mouse = {
    x: null,
    y: null,
    radius: 120
  };

  window.addEventListener("mousemove", function(event) {
    mouse.x = event.x;
    mouse.y = event.y;
  });

  window.addEventListener("resize", function() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    init();
  });

  class Particle {
    constructor(x, y, directionX, directionY) {
      this.x = x;
      this.y = y;
      this.directionX = directionX;
      this.directionY = directionY;
    }

    draw() {
      ctx.beginPath();
      ctx.strokeStyle = "rgba(255,255,255,0.15)";
      ctx.lineWidth = 0.5;
      ctx.moveTo(this.x, this.y);
      ctx.lineTo(this.x + this.directionX * 8, this.y + this.directionY * 8);
      ctx.stroke();
    }

    update() {
      if (this.x > canvas.width || this.x < 0) {
        this.directionX = -this.directionX;
      }
      if (this.y > canvas.height || this.y < 0) {
        this.directionY = -this.directionY;
      }

      let dx = mouse.x - this.x;
      let dy = mouse.y - this.y;
      let distance = Math.sqrt(dx * dx + dy * dy);

      if (distance < mouse.radius) {
        this.x -= dx * 0.02;
        this.y -= dy * 0.02;
      }

      this.x += this.directionX;
      this.y += this.directionY;

      this.draw();
    }
  }

  function init() {
    particles = [];
    let numberOfParticles = 150;

    for (let i = 0; i < numberOfParticles; i++) {
      let x = Math.random() * canvas.width;
      let y = Math.random() * canvas.height;
      let directionX = (Math.random() - 0.5) * 0.5;
      let directionY = (Math.random() - 0.5) * 0.5;

      particles.push(new Particle(x, y, directionX, directionY));
    }
  }

  function animate() {
    requestAnimationFrame(animate);
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    particles.forEach(particle => particle.update());
  }

  init();
  animate();
</script>

</body>
</html>
