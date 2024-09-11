https://editor.p5js.org/PucciCLT/full/wNljgVn4i
.

let player;
let obstacles = [];
let score = 0;
let obstacleSpeed = 3; // Velocidade inicial dos obstáculos
let speedIncreaseInterval = 1000; // Intervalo de aumento de velocidade em frames
let lastIncreaseTime = 0; // Tempo do último aumento de velocidade

function setup() {
  createCanvas(800, 600);
  player = new Player();
}

function draw() {
  background(200);

  // Atualiza e desenha o jogador
  player.update();
  player.display();

  // Atualiza e desenha obstáculos
  if (frameCount % 60 === 0) { // Adiciona um novo obstáculo a cada segundo
    obstacles.push(new Obstacle());
  }

  for (let i = obstacles.length - 1; i >= 0; i--) {
    obstacles[i].update();
    obstacles[i].display();

    // Verifica colisão com o jogador
    if (player.hits(obstacles[i])) {
      noLoop(); // Para o jogo quando o jogador colide com um obstáculo
      textSize(32);
      fill(255, 0, 0);
      textAlign(CENTER, CENTER);
      text("Game Over", width / 2, height / 2);
      return;
    }

    // Remove obstáculos que saíram da tela
    if (obstacles[i].offscreen()) {
      obstacles.splice(i, 1);
      score++;
    }
  }

  // Atualiza a velocidade dos obstáculos a cada intervalo
  if (frameCount - lastIncreaseTime >= speedIncreaseInterval) {
    obstacleSpeed += 0.5; // Aumenta a velocidade dos obstáculos
    lastIncreaseTime = frameCount; // Atualiza o tempo do último aumento
  }

  // Mostra a pontuação
  textSize(16);
  fill(0);
  textAlign(LEFT, TOP);
  text("Score: " + score, 10, 10);
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    player.move(-1); // Move para a esquerda
  } else if (keyCode === RIGHT_ARROW) {
    player.move(1); // Move para a direita
  }
}

function keyReleased() {
  if (keyCode === LEFT_ARROW || keyCode === RIGHT_ARROW) {
    player.move(0); // Para o movimento quando a tecla é solta
  }
}

class Player {
  constructor() {
    this.x = width / 2;
    this.y = height - 30;
    this.size = 50;
    this.speed = 5;
    this.velocity = 0; // Velocidade de movimento
  }

  update() {
    this.x += this.velocity * this.speed;

    // Limita a movimentação do jogador dentro da tela
    this.x = constrain(this.x, 0, width - this.size);
  }

  move(direction) {
    this.velocity = direction;
  }

  display() {
    fill(0, 0, 255);
    noStroke();
    rect(this.x, this.y, this.size, this.size);
  }

  hits(obstacle) {
    return !(this.x > obstacle.x + obstacle.size ||
             this.x + this.size < obstacle.x ||
             this.y > obstacle.y + obstacle.size ||
             this.y + this.size < obstacle.y);
  }
}

class Obstacle {
  constructor() {
    this.x = random(width);
    this.y = -50;
    this.size = 50;
    this.speed = obstacleSpeed; // Usa a velocidade global
  }

  update() {
    this.y += this.speed;
  }

  display() {
    fill(255, 0, 0);
    noStroke();
    rect(this.x, this.y, this.size, this.size);
  }

  offscreen() {
    return this.y > height;
  }
}
