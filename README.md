# Subwaminigame
Um jogo parecido com o subway surfers

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>Runner 2D</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #080d19;
    color: white;
    font-family: Arial, sans-serif;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 12px;
}

#game-container {
    position: relative;
    width: min(100%, 440px);
    background: #10192b;
    border: 1px solid #354966;
    border-radius: 18px;
    overflow: hidden;
    box-shadow: 0 0 35px #00c8ff20;
}

#topbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 8px;
    padding: 12px;
    background: linear-gradient(110deg, #142b43, #10172a);
}

#topbar h2 {
    font-size: 16px;
    margin: 0;
    letter-spacing: 2px;
    color: #6ceaff;
}

#fullscreen {
    background: #203e5b;
    border: 1px solid #4c7798;
    color: white;
    padding: 9px 11px;
    border-radius: 9px;
    font-weight: bold;
    cursor: pointer;
}

#fullscreen:hover {
    background: #315c7f;
}

#stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 6px;
    padding: 8px;
    background: #0c1423;
}

.stat {
    text-align: center;
    background: #16243a;
    border: 1px solid #263d59;
    border-radius: 9px;
    padding: 8px 3px;
}

.stat small {
    display: block;
    color: #9cb4d0;
    font-size: 10px;
    margin-bottom: 4px;
    letter-spacing: 1px;
}

.stat strong {
    font-size: 16px;
    color: #fff;
}

#stage {
    position: relative;
    width: 100%;
    background: #07131e;
    line-height: 0;
    touch-action: pan-y;
}

canvas {
    width: 100%;
    height: auto;
    display: block;
    aspect-ratio: 400 / 600;
}

#message {
    position: absolute;
    inset: 0;
    display: none;
    align-items: center;
    justify-content: center;
    background: #020817bd;
    backdrop-filter: blur(5px);
    text-align: center;
    padding: 24px;
    line-height: normal;
}

#message .panel {
    width: 100%;
    max-width: 300px;
    padding: 24px 18px;
    border-radius: 18px;
    border: 1px solid #47789a;
    background: linear-gradient(145deg, #172b43f5, #091222f5);
    box-shadow: 0 0 35px #00c8ff22;
}

#message h1 {
    font-size: 30px;
    margin: 0 0 12px;
    color: #ff657e;
}

#message p {
    color: #d4e7ff;
    line-height: 1.5;
}

#restart {
    padding: 13px 22px;
    border: 0;
    border-radius: 10px;
    background: linear-gradient(100deg, #00bde8, #4778ff);
    color: white;
    font-weight: bold;
    font-size: 15px;
    cursor: pointer;
}

#controls {
    display: flex;
    justify-content: center;
    gap: 15px;
    padding: 12px;
    background: #0c1423;
}

.move {
    width: 100px;
    height: 45px;
    border-radius: 12px;
    border: 1px solid #3b607f;
    background: #192d47;
    color: white;
    font-size: 23px;
    touch-action: manipulation;
    cursor: pointer;
}

.move:active {
    background: #28608a;
}

#help {
    text-align: center;
    padding: 0 8px 12px;
    background: #0c1423;
    color: #8da7c5;
    font-size: 11px;
    line-height: 1.5;
}

/* Tela cheia apenas para o jogo */
#game-container:fullscreen {
    width: 100%;
    height: 100%;
    max-width: none;
    border: none;
    border-radius: 0;
    background: #07131e;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

#game-container:fullscreen #topbar {
    flex-shrink: 0;
}

#game-container:fullscreen #stats {
    flex-shrink: 0;
}

#game-container:fullscreen #stage {
    flex: 1;
    min-height: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
}

#game-container:fullscreen canvas {
    width: 100%;
    height: 100%;
    max-width: calc((100vh - 130px) * 2 / 3);
    object-fit: contain;
}

#game-container:fullscreen #controls,
#game-container:fullscreen #help {
    flex-shrink: 0;
}

@media (max-width: 380px) {
    #topbar h2 {
        font-size: 13px;
    }

    .stat strong {
        font-size: 14px;
    }
}
</style>
</head>

<body>

<div id="game-container">

    <div id="topbar">
        <h2>⚡ RUNNER 2D</h2>
        <button id="fullscreen">⛶ Tela cheia</button>
    </div>

    <div id="stats">
        <div class="stat">
            <small>PONTOS</small>
            <strong id="score">0</strong>
        </div>

        <div class="stat">
            <small>MOEDAS</small>
            <strong id="coins">0</strong>
        </div>

        <div class="stat">
            <small>NO COIN</small>
            <strong id="timer">00:00.000</strong>
        </div>
    </div>

    <div id="stage">
        <canvas id="game" width="800" height="1200"></canvas>

        <div id="message">
            <div class="panel">
                <h1>GAME OVER</h1>
                <p id="result">Você bateu em um obstáculo!</p>
                <button id="restart">↻ Jogar novamente</button>
            </div>
        </div>
    </div>

    <div id="controls">
        <button class="move" id="left">◀</button>
        <button class="move" id="right">▶</button>
    </div>

    <div id="help">
        Use as setas ou os botões para trocar de trilho.<br>
        Desvie dos obstáculos e colete as moedas!
    </div>

</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const W = 400;
const H = 600;

ctx.scale(2, 2);

const $ = id => document.getElementById(id);

const laneX = [125, 200, 275];

let playerLane;
let playerX;
let obstacles;
let coins;
let particles;

let score;
let coinCount;
let speed;
let spawnTimer;
let coinTimer;
let sceneryOffset;
let gameOver;
let started;

let startTime;
let finalTime;
let coinCollected;
let lastFrame;

const rand = (min, max) => Math.random() * (max - min) + min;

function resetGame() {
    playerLane = 1;
    playerX = laneX[playerLane];

    obstacles = [];
    coins = [];
    particles = [];

    score = 0;
    coinCount = 0;
    speed = 190;

    spawnTimer = 0.8;
    coinTimer = 1.5;
    sceneryOffset = 0;

    gameOver = false;
    started = true;

    startTime = performance.now();
    finalTime = null;
    coinCollected = false;

    lastFrame = performance.now();

    $("message").style.display = "none";

    updateStats();
}

function formatTime(ms) {
    ms = Math.max(0, ms);

    const minutes = Math.floor(ms / 60000);
    const seconds = Math.floor((ms % 60000) / 1000);
    const millis = Math.floor(ms % 1000);

    return String(minutes).padStart(2, "0") + ":" +
           String(seconds).padStart(2, "0") + "." +
           String(millis).padStart(3, "0");
}

function updateStats() {
    $("score").textContent = Math.floor(score);
    $("coins").textContent = coinCount;

    const elapsed = finalTime !== null
        ? finalTime
        : performance.now() - startTime;

    $("timer").textContent = formatTime(elapsed);
}

function movePlayer(direction) {
    if (gameOver) return;

    playerLane = Math.max(0, Math.min(2, playerLane + direction));
}

document.addEventListener("keydown", e => {
    if (["ArrowLeft", "ArrowRight", " "].includes(e.key)) {
        e.preventDefault();
    }

    if (e.key === "ArrowLeft" || e.key === "a") {
        movePlayer(-1);
    }

    if (e.key === "ArrowRight" || e.key === "d") {
        movePlayer(1);
    }

    if (e.key.toLowerCase() === "r" && gameOver) {
        resetGame();
    }
});

$("left").addEventListener("pointerdown", e => {
    e.preventDefault();
    movePlayer(-1);
});

$("right").addEventListener("pointerdown", e => {
    e.preventDefault();
    movePlayer(1);
});

$("restart").addEventListener("click", resetGame);

$("fullscreen").addEventListener("click", async () => {
    const container = $("game-container");

    try {
        if (!document.fullscreenElement) {
            await container.requestFullscreen();

            if (screen.orientation && screen.orientation.lock) {
                try {
                    await screen.orientation.lock("landscape");
                } catch (_) {}
            }
        } else {
            await document.exitFullscreen();
        }
    } catch (err) {
        console.log("Tela cheia não disponível neste navegador.");
    }
});

document.addEventListener("fullscreenchange", () => {
    $("fullscreen").textContent =
        document.fullscreenElement ? "✕ Sair da tela cheia" : "⛶ Tela cheia";
});

let touchStartX = 0;
let touchStartY = 0;

canvas.addEventListener("touchstart", e => {
    const t = e.changedTouches[0];
    touchStartX = t.clientX;
    touchStartY = t.clientY;
}, { passive: true });

canvas.addEventListener("touchend", e => {
    const t = e.changedTouches[0];
    const dx = t.clientX - touchStartX;
    const dy = t.clientY - touchStartY;

    if (Math.abs(dx) > 25 && Math.abs(dx) > Math.abs(dy)) {
        movePlayer(dx < 0 ? -1 : 1);
    }
}, { passive: true });

function roundedRect(x, y, w, h, r, color) {
    ctx.fillStyle = color;
    ctx.beginPath();
    ctx.roundRect(x, y, w, h, r);
    ctx.fill();
}

/* CÉU E MONTANHAS */

function drawBackground() {
    const sky = ctx.createLinearGradient(0, 0, 0, H);
    sky.addColorStop(0, "#07152d");
    sky.addColorStop(0.42, "#245879");
    sky.addColorStop(0.72, "#78b5a5");
    sky.addColorStop(1, "#244b42");

    ctx.fillStyle = sky;
    ctx.fillRect(0, 0, W, H);

    // Brilho do horizonte
    const glow = ctx.createRadialGradient(200, 270, 5, 200, 270, 210);
    glow.addColorStop(0, "#d7f7d966");
    glow.addColorStop(1, "#d7f7d900");

    ctx.fillStyle = glow;
    ctx.fillRect(0, 100, W, 350);

    // Sol
    const sun = ctx.createRadialGradient(290, 135, 5, 290, 135, 42);
    sun.addColorStop(0, "#fff5ce");
    sun.addColorStop(0.6, "#ffe3a6");
    sun.addColorStop(1, "#ffd08a00");

    ctx.fillStyle = sun;
    ctx.beginPath();
    ctx.arc(290, 135, 42, 0, Math.PI * 2);
    ctx.fill();

    // Montanhas distantes
    drawMountainLayer("#557c89", 235, 38, 65, 0.12);
    drawMountainLayer("#315f68", 275, 48, 85, 0.24);

    // Vegetação no horizonte
    ctx.fillStyle = "#244d43";
    ctx.beginPath();
    ctx.moveTo(0, 290);

    for (let x = 0; x <= W; x += 18) {
        const y = 275 + Math.sin(x * 0.035) * 13;
        ctx.lineTo(x, y);
    }

    ctx.lineTo(W, H);
    ctx.lineTo(0, H);
    ctx.closePath();
    ctx.fill();
}

function drawMountainLayer(color, baseY, amplitude, width, factor) {
    const shift = (sceneryOffset * factor) % width;

    ctx.fillStyle = color;
    ctx.beginPath();
    ctx.moveTo(-width, baseY + 100);

    for (let x = -width; x <= W + width; x += width / 2) {
        const xx = x - shift;
        const peak = baseY - amplitude * (0.5 + 0.5 * Math.sin(x * 0.04));
        ctx.lineTo(xx, peak);
    }

    ctx.lineTo(W + width, H);
    ctx.lineTo(-width, H);
    ctx.closePath();
    ctx.fill();
}

/* ESTRADA COM PROFUNDIDADE */

function roadEdges(y) {
    const t = Math.max(0, Math.min(1, (y - 255) / 345));
    const half = 30 + t * 170;

    return {
        left: W / 2 - half,
        right: W / 2 + half,
        half,
        t
    };
}

function drawRoad() {
    const horizonY = 255;

    // Chão
    ctx.fillStyle = "#1e493b";
    ctx.fillRect(0, horizonY, W, H - horizonY);

    // Faixas laterais da estrada
    ctx.beginPath();
    ctx.moveTo(200 - 30, horizonY);
    ctx.lineTo(200 + 30, horizonY);
    ctx.lineTo(370, H);
    ctx.lineTo(30, H);
    ctx.closePath();

    const road = ctx.createLinearGradient(0, horizonY, 0, H);
    road.addColorStop(0, "#40505c");
    road.addColorStop(1, "#17212d");

    ctx.fillStyle = road;
    ctx.fill();

    // Bordas iluminadas
    ctx.strokeStyle = "#b8d6d8";
    ctx.lineWidth = 2;

    ctx.beginPath();
    ctx.moveTo(170, horizonY);
    ctx.lineTo(30, H);
    ctx.moveTo(230, horizonY);
    ctx.lineTo(370, H);
    ctx.stroke();

    // Linhas dos 3 trilhos
    for (let lane = 1; lane <= 2; lane++) {
        const topX = 200 + (lane - 1.5) * 20;
        const bottomX = 200 + (lane - 1.5) * 113;

        ctx.strokeStyle = "#c3d8e1a0";
        ctx.lineWidth = 1.5;

        ctx.beginPath();
        ctx.moveTo(topX, horizonY);
        ctx.lineTo(bottomX, H);
        ctx.stroke();
    }

    // Marcas da pista em perspectiva
    const period = 65;
    const offset = sceneryOffset % period;

    for (let i = -1; i < 12; i++) {
        const y = horizonY + ((i * period + offset) % (H - horizonY));

        if (y < horizonY) continue;

        const { t, half } = roadEdges(y);
        const markHeight = 2 + t * 12;
        const markWidth = 1 + t * 3;

        ctx.fillStyle = "#d9e5e7c9";

        for (const lane of [1, 2]) {
            const x = 200 + (lane - 1.5) * half;

            ctx.fillRect(
                x - markWidth / 2,
                y,
                markWidth,
                markHeight
            );
        }
    }
}

/* ÁRVORES ANIMADAS NAS LATERAIS */

function drawTree(x, y, size) {
    const s = size;

    // Tronco
    ctx.fillStyle = "#49392d";
    ctx.fillRect(x - s * 0.09, y, s * 0.18, s * 0.48);

    // Folhagem
    const gradient = ctx.createLinearGradient(
        x - s / 2, y - s, x + s / 2, y
    );

    gradient.addColorStop(0, "#79c98c");
    gradient.addColorStop(0.5, "#24764d");
    gradient.addColorStop(1, "#123e39");

    ctx.fillStyle = gradient;

    ctx.beginPath();
    ctx.moveTo(x, y - s);
    ctx.lineTo(x - s * 0.42, y - s * 0.22);
    ctx.lineTo(x - s * 0.22, y - s * 0.22);
    ctx.lineTo(x - s * 0.52, y + s * 0.05);
    ctx.lineTo(x + s * 0.52, y + s * 0.05);
    ctx.lineTo(x + s * 0.22, y - s * 0.22);
    ctx.lineTo(x + s * 0.42, y - s * 0.22);
    ctx.closePath();
    ctx.fill();
}

function drawScenery() {
    const period = 80;
    const offset = sceneryOffset % period;

    for (let i = -1; i < 10; i++) {
        const y = 260 + ((i * period + offset) % 390);

        if (y < 260) continue;

        const t = (y - 260) / 340;
        const size = 8 + t * 36;

        const edge = roadEdges(y);

        drawTree(
            edge.left - size * 0.55 - 5,
            y,
            size
        );

        drawTree(
            edge.right + size * 0.55 + 5,
            y,
            size
        );
    }
}

/* JOGADOR */

function drawPlayer() {
    const targetX = laneX[playerLane];

    playerX += (targetX - playerX) * 0.22;

    const run = Math.sin(performance.now() / 75) * 3;

    // Sombra
    ctx.fillStyle = "#00000055";
    ctx.beginPath();
    ctx.ellipse(playerX, 531, 20, 8, 0, 0, Math.PI * 2);
    ctx.fill();

    // Aura
    const aura = ctx.createRadialGradient(
        playerX, 500, 2,
        playerX, 500, 36
    );

    aura.addColorStop(0, "#35dfff45");
    aura.addColorStop(1, "#35dfff00");

    ctx.fillStyle = aura;
    ctx.fillRect(playerX - 38, 455, 76, 90);

    // Pernas
    roundedRect(playerX - 12, 504 + run, 9, 25, 4, "#15284e");
    roundedRect(playerX + 3, 504 - run, 9, 25, 4, "#15284e");

    // Tênis
    roundedRect(playerX - 15, 525 + run, 13, 7, 3, "#e9f7ff");
    roundedRect(playerX + 2, 525 - run, 13, 7, 3, "#e9f7ff");

    // Corpo
    const body = ctx.createLinearGradient(
        playerX - 15, 465, playerX + 15, 505
    );

    body.addColorStop(0, "#66f0ff");
    body.addColorStop(1, "#2875e8");

    roundedRect(playerX - 15, 468, 30, 38, 9, body);

    // Braços
    ctx.strokeStyle = "#f2c6a2";
    ctx.lineWidth = 7;
    ctx.lineCap = "round";

    ctx.beginPath();
    ctx.moveTo(playerX - 13, 477);
    ctx.lineTo(playerX - 20, 492 + run);
    ctx.moveTo(playerX + 13, 477);
    ctx.lineTo(playerX + 20, 492 - run);
    ctx.stroke();

    // Pescoço
    ctx.fillStyle = "#f2c6a2";
    ctx.fillRect(playerX - 5, 460, 10, 12);

    // Cabeça
    const skin = ctx.createLinearGradient(
        playerX - 12, 435, playerX + 12, 465
    );

    skin.addColorStop(0, "#ffd9b5");
    skin.addColorStop(1, "#d99b77");

    ctx.fillStyle = skin;
    ctx.beginPath();
    ctx.arc(playerX, 450, 13, 0, Math.PI * 2);
    ctx.fill();

    // Cabelo
    ctx.fillStyle = "#293044";
    ctx.beginPath();
    ctx.arc(playerX, 445, 13, Math.PI, Math.PI * 2);
    ctx.lineTo(playerX + 11, 449);
    ctx.lineTo(playerX - 11, 449);
    ctx.closePath();
    ctx.fill();

    // Visor
    roundedRect(playerX - 10, 448, 20, 5, 2, "#253c55");
}

/* OBSTÁCULOS */

function drawObstacle(o) {
    const scale = 0.45 + (o.y / H) * 0.65;
    const w = 34 * scale;
    const h = 34 * scale;

    const x = laneX[o.lane];
    const y = o.y;

    // Sombra
    ctx.fillStyle = "#00000055";
    ctx.beginPath();
    ctx.ellipse(x, y + h * 0.42, w * 0.7, h * 0.22, 0, 0, Math.PI * 2);
    ctx.fill();

    // Brilho de aviso
    ctx.fillStyle = "#ff526622";
    ctx.beginPath();
    ctx.arc(x, y, w * 0.95, 0, Math.PI * 2);
    ctx.fill();

    // Caixa
    const g = ctx.createLinearGradient(x - w / 2, y - h / 2, x + w / 2, y + h / 2);
    g.addColorStop(0, "#ff7d72");
    g.addColorStop(0.5, "#d93654");
    g.addColorStop(1, "#762440");

    roundedRect(x - w / 2, y - h / 2, w, h, 5 * scale, g);

    // Borda
    ctx.strokeStyle = "#ffb1a7";
    ctx.lineWidth = 1.5 * scale;
    ctx.strokeRect(x - w / 2, y - h / 2, w, h);

    // Faixa de perigo
    ctx.save();
    ctx.beginPath();
    ctx.rect(x - w / 2, y - h / 2, w, h);
    ctx.clip();

    ctx.strokeStyle = "#ffd8a0";
    ctx.lineWidth = 4 * scale;

    for (let i = -2; i < 4; i++) {
        ctx.beginPath();
        ctx.moveTo(x - w / 2 + i * 12 * scale, y + h / 2);
        ctx.lineTo(x - w / 2 + (i + 1.2) * 12 * scale, y - h / 2);
        ctx.stroke();
    }

    ctx.restore();
}

/* MOEDAS */

function drawCoin(c) {
    const x = laneX[c.lane];
    const y = c.y;

    const scale = 0.5 + (y / H) * 0.5;
    const r = 10 * scale;

    const pulse = 1 + Math.sin(performance.now() / 120 + c.phase) * 0.08;

    ctx.save();
    ctx.translate(x, y);
    ctx.scale(pulse, 1);

    // Brilho
    const glow = ctx.createRadialGradient(0, 0, 1, 0, 0, r * 2.8);
    glow.addColorStop(0, "#fff4a988");
    glow.addColorStop(1, "#ffd70000");

    ctx.fillStyle = glow;
    ctx.beginPath();
    ctx.arc(0, 0, r * 2.8, 0, Math.PI * 2);
    ctx.fill();

    // Borda externa
    ctx.fillStyle = "#ff9f16";
    ctx.beginPath();
    ctx.arc(0, 0, r, 0, Math.PI * 2);
    ctx.fill();

    // Centro
    const gold = ctx.createRadialGradient(-r * 0.3, -r * 0.3, 1, 0, 0, r);
    gold.addColorStop(0, "#fff8a7");
    gold.addColorStop(0.6, "#ffdc38");
    gold.addColorStop(1, "#ff9d00");

    ctx.fillStyle = gold;
    ctx.beginPath();
    ctx.arc(0, 0, r * 0.78, 0, Math.PI * 2);
    ctx.fill();

    ctx.strokeStyle = "#fff7b5";
    ctx.lineWidth = 1.5 * scale;
    ctx.stroke();

    ctx.fillStyle = "#b8790d";
    ctx.font = `bold ${13 * scale}px Arial`;
    ctx.textAlign = "center";
    ctx.textBaseline = "middle";
    ctx.fillText("$", 0, 0);

    ctx.restore();
}

/* PARTÍCULAS */

function createParticles(x, y, color, amount = 10) {
    for (let i = 0; i < amount; i++) {
        particles.push({
            x,
            y,
            vx: rand(-90, 90),
            vy: rand(-140, 20),
            life: rand(0.3, 0.7),
            maxLife: 0.7,
            size: rand(2, 5),
            color
        });
    }
}

function updateParticles(dt) {
    for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i];

        p.x += p.vx * dt;
        p.y += p.vy * dt;
        p.vy += 180 * dt;
        p.life -= dt;

        if (p.life <= 0) {
            particles.splice(i, 1);
        }
    }
}

function drawParticles() {
    for (const p of particles) {
        ctx.globalAlpha = Math.max(0, p.life / p.maxLife);
        ctx.fillStyle = p.color;

        ctx.beginPath();
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        ctx.fill();
    }

    ctx.globalAlpha = 1;
}

/* LÓGICA */

function spawnObstacle() {
    const lane = Math.floor(Math.random() * 3);

    obstacles.push({
        lane,
        y: 270,
        passed: false
    });
}

function spawnCoin() {
    let lane = Math.floor(Math.random() * 3);

    // Evita colocar a moeda no mesmo trilho de um obstáculo muito próximo.
    for (let attempt = 0; attempt < 5; attempt++) {
        const blocked = obstacles.some(
            o => o.lane === lane && o.y < 340 && o.y > 220
        );

        if (!blocked) break;

        lane = Math.floor(Math.random() * 3);
    }

    coins.push({
        lane,
        y: 270,
        phase: rand(0, Math.PI * 2)
    });
}

function endGame() {
    if (gameOver) return;

    gameOver = true;

    finalTime = performance.now() - startTime;

    $("result").textContent =
        `Pontuação: ${Math.floor(score)} | Moedas: ${coinCount} | NO COIN: ${formatTime(finalTime)}`;

    $("message").style.display = "flex";
}

function update(dt) {
    if (!started || gameOver) return;

    // Limita a velocidade do cenário para evitar saltos grandes.
    dt = Math.min(dt, 0.035);

    speed = Math.min(340, speed + 2.2 * dt);
    sceneryOffset += speed * dt;

    score += dt * 10;

    spawnTimer -= dt;
    coinTimer -= dt;

    if (spawnTimer <= 0) {
        spawnObstacle();
        spawnTimer = rand(0.8, 1.35) * Math.max(0.58, 190 / speed);
    }

    if (coinTimer <= 0) {
        spawnCoin();
        coinTimer = rand(1.3, 2.2);
    }

    for (let i = obstacles.length - 1; i >= 0; i--) {
        const o = obstacles[i];

        o.y += speed * dt;

        // Colisão com o jogador
        if (
            o.lane === playerLane &&
            Math.abs(o.y - 510) < 23
        ) {
            endGame();
            return;
        }

        if (o.y > 630) {
            obstacles.splice(i, 1);
        }
    }

    for (let i = coins.length - 1; i >= 0; i--) {
        const c = coins[i];

        c.y += speed * dt;

        if (
            c.lane === playerLane &&
            Math.abs(c.y - 510) < 25
        ) {
            coinCount++;

            createParticles(laneX[c.lane], c.y, "#ffdf48", 16);

            // O cronômetro NO COIN para na primeira moeda.
            if (!coinCollected) {
                finalTime = performance.now() - startTime;
                coinCollected = true;
            }

            coins.splice(i, 1);
            continue;
        }

        if (c.y > 630) {
            coins.splice(i, 1);
        }
    }

    updateParticles(dt);
    updateStats();
}

/* RENDERIZAÇÃO */

function render() {
    ctx.clearRect(0, 0, W, H);

    drawBackground();
    drawRoad();
    drawScenery();

    // Elementos mais distantes primeiro.
    for (const c of coins) {
        drawCoin(c);
    }

    for (const o of obstacles) {
        drawObstacle(o);
    }

    drawPlayer();
    drawParticles();

    // Vinheta para dar profundidade.
    const vignette = ctx.createRadialGradient(
        200, 300, 150,
        200, 300, 410
    );

    vignette.addColorStop(0, "#00000000");
    vignette.addColorStop(1, "#02081470");

    ctx.fillStyle = vignette;
    ctx.fillRect(0, 0, W, H);

    // Pequena indicação visual do trilho atual.
    ctx.fillStyle = "#7beaff";
    ctx.globalAlpha = 0.7;

    ctx.beginPath();
    ctx.moveTo(playerX, 548);
    ctx.lineTo(playerX - 6, 558);
    ctx.lineTo(playerX + 6, 558);
    ctx.closePath();
    ctx.fill();

    ctx.globalAlpha = 1;
}

function loop(now) {
    const dt = (now - lastFrame) / 1000;
    lastFrame = now;

    update(dt);
    render();

    requestAnimationFrame(loop);
}

resetGame();
requestAnimationFrame(loop);
</script>

</body>
</html>
```
