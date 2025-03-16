# game
const canvas = document.createElement("canvas");
const ctx = canvas.getContext("2d");
document.body.appendChild(canvas);

canvas.width = 400;
canvas.height = 600;

let bird = { x: 50, y: 300, width: 20, height: 20, velocity: 0, gravity: 0.5, lift: -8 };
let pipes = [];
let frame = 0;
let score = 0;
let level = 1;
let boundary = { top: 0, bottom: canvas.height };

document.addEventListener("keydown", () => bird.velocity = bird.lift);

function update() {
    bird.velocity += bird.gravity;
    bird.y += bird.velocity;

    if (bird.y + bird.height > boundary.bottom || bird.y < boundary.top) location.reload();

    if (frame % 75 === 0) pipes.push({ x: canvas.width, y: Math.random() * (canvas.height - 150) + 50, width: 40, gap: 120 });

    pipes.forEach(pipe => {
        pipe.x -= 2;
        if (pipe.x + pipe.width < 0) {
            pipes.shift();
            score++;
            if (score % 10 === 0) {
                level++;
                boundary.top += 10;
                boundary.bottom -= 10;
            }
        }
    });

    draw();
    frame++;
    requestAnimationFrame(update);
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "black";
    ctx.fillRect(bird.x, bird.y, bird.width, bird.height);

    pipes.forEach(pipe => {
        ctx.fillStyle = "green";
        ctx.fillRect(pipe.x, 0, pipe.width, pipe.y);
        ctx.fillRect(pipe.x, pipe.y + pipe.gap, pipe.width, canvas.height - pipe.y - pipe.gap);
    });

    ctx.fillStyle = "red";
    ctx.fillRect(0, boundary.top, canvas.width, 2);
    ctx.fillRect(0, boundary.bottom, canvas.width, 2);

    ctx.fillStyle = "black";
    ctx.fillText(`Score: ${score}`, 10, 20);
    ctx.fillText(`Level: ${level}`, 10, 40);
}

update();
