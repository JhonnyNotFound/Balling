This Code is INSANE!
let cor;
let posicaoHorizontal // x
let posicaoVertical // y

function setup() {
  createCanvas(400, 400);
  background(color(100,0,0));
  cor = color(random(0, 255), random(0, 255), random(0, 255));
  posicaoHorizontal = 200
  posicaoVertical = 200
}


function draw() {
  
  fill(cor);
  circle(posicaoHorizontal, posicaoVertical, 50);
  
  if(mouseX < posicaoHorizontal) {
    posicaoHorizontal--;
    
    
  }
  
  if(mouseX > posicaoHorizontal) {
    posicaoHorizontal++;
  }
  
  if(mouseY < posicaoVertical) {
    posicaoVertical--;
  }
  
  if(mouseY > posicaoVertical) {
    posicaoVertical++;
  }
  if(mouseIsPressed){
    cor = color(random(0, 255), random(0, 255), random(0, 255));
}
function draw() {
    background(0);
    mostraBolinha();
    movimentaBolinha();
    verificaColisaoBorda();
    rect(5, 150, 10, 90);
}

  
}
