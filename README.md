// ============ Create Snow ============
const NUM = 150;
let snowflakes = [];

function createSnow() {
    for (let i = 0; i < NUM; i++) {
        let size = Math.random() * 8 + 4; // 更自然的小雪花 4~12px
        snowflakes.push({
            img: images[Math.floor(Math.random() * images.length)],
            x: Math.random() * canvas.width,
            y: canvas.height - Math.random() * 60,  // 底部松散堆积
            size: size,
            vx: (Math.random() - 0.5) * 0.3,        // 随机初速度
            vy: Math.random() * 0.3
        });
    }
}
createSnow();


function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    snowflakes.forEach(flake => {
    
        flake.vx += tiltX * 0.02;
        flake.vy += tiltY * 0.03;

        flake.vx += (Math.random() - 0.5) * 0.05;
        flake.vy += (Math.random() - 0.5) * 0.02;

        // ------ 摩擦阻尼，让雪稳定下来 ------
        flake.vx *= 0.98;
        flake.vy *= 0.985;

        flake.x += flake.vx;
        flake.y += flake.vy;

        // ------ 边界：像水晶球的玻璃壁一样反弹 ------
        if (flake.x < 0) { flake.x = 0; flake.vx *= -0.4; }
        if (flake.x > canvas.width - flake.size) {
            flake.x = canvas.width - flake.size;
            flake.vx *= -0.4;
        }

        if (flake.y > canvas.height - flake.size) {
            flake.y = canvas.height - flake.size;
            flake.vy *= -0.4;
        }
        if (flake.y < canvas.height * 0.1) {
            flake.y = canvas.height * 0.1;
            flake.vy *= -0.3;
        }

        // ------ 绘制雪花 ------
        ctx.drawImage(flake.img, flake.x, flake.y, flake.size, flake.size);
    });

    requestAnimationFrame(draw);
}
