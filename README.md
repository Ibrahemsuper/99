<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Full Screen Threads</title>
<style>
    body {
        margin: 0;
        overflow: hidden;
        background: #0f0f1a;
    }
    canvas {
        display: block;
    }
</style>
</head>
<body>

<canvas id="bg"></canvas>

<script>
const canvas = document.getElementById("bg");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let particles = [];
let mouse = { x: null, y: null };

window.addEventListener("mousemove", (e) => {
    mouse.x = e.x;
    mouse.y = e.y;
});

window.addEventListener("resize", () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    init(); // 🔥 عدلت هون لحتى يرجع يعبّي الشاشة عند التكبير
});

class Particle {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.size = 2;
        this.baseX = this.x;
        this.baseY = this.y;
        this.density = Math.random() * 30 + 1;
    }

    draw() {
        ctx.fillStyle = "#00ffff";
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fill();
    }

    update() {
        let dx = mouse.x - this.x;
        let dy = mouse.y - this.y;
        let distance = Math.sqrt(dx * dx + dy * dy);

        if (distance < 120) { // 🔥 عدلت المسافة هون (كانت 100)
            this.x -= dx / 15;
            this.y -= dy / 15;
        } else {
            this.x += (this.baseX - this.x) * 0.05;
            this.y += (this.baseY - this.y) * 0.05;
        }
    }
}

function init() {
    particles = [];
    for (let y = 0; y < canvas.height; y += 15) { // 🔥 كانت 25 خليتها 15
        for (let x = 0; x < canvas.width; x += 15) { // 🔥 كانت 25 خليتها 15
            particles.push(new Particle(x, y));
        }
    }
}

function connect() {
    for (let a = 0; a < particles.length; a++) {
        for (let b = a; b < particles.length; b++) {
            let dx = particles[a].x - particles[b].x;
            let dy = particles[a].y - particles[b].y;
            let distance = dx * dx + dy * dy;

            if (distance < 400) { // 🔥 كانت 900 وعدلتها لتناسب الكثافة
                ctx.strokeStyle = "rgba(0,255,255,0.15)";
                ctx.lineWidth = 1;
                ctx.beginPath();
                ctx.moveTo(particles[a].x, particles[a].y);
                ctx.lineTo(particles[b].x, particles[b].y);
                ctx.stroke();
            }
        }
    }
}

function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    particles.forEach(p => {
        p.update();
        p.draw();
    });

    connect();
    requestAnimationFrame(animate);
}

init();
animate();
</script>

</body>
</html>





update() {
    let dx = mouse.x - this.x;
    let dy = mouse.y - this.y;
    let distance = Math.sqrt(dx * dx + dy * dy);

    let maxDistance = 120;

    if (distance < maxDistance) {
        let force = (maxDistance - distance) / maxDistance;
        this.x -= dx * force * 0.02;   // 🔥 كانت قوية كتير
        this.y -= dy * force * 0.02;   // خففناها
    } else {
        // رجعة ناعمة جداً
        this.x += (this.baseX - this.x) * 0.03;
        this.y += (this.baseY - this.y) * 0.03;
    }
}
