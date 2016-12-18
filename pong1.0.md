# POng-Processing
//Pong 1.0//
//Alberto Yúfera Daza 2ºBachilleratoB//
//Diciembre 2016//

int pantalla=0;
int instrucciones=0;

int vidas=3;
int puntos=0;

//colores de fondo//
int Rback=0;
int Bback=127;

//bola
int Rb=20;

int Xbola=(width/2);
int Ybola=(height/2);

PVector velocidadbola;
PVector posicionbola;
PVector aceleracionbola;

//pala
int largopala=150;
int anchopala=15;

int Xpala=250;
int Ypala=550;

int VXpala=0;
int VYpala=0;

//botón start//
int anchoboton1=100;
int largoboton1=300;
int Xboton1=370;
int Yboton1=220;

//botón reinicio//
int anchoboton2=100;
int largoboton2=300;
int Xboton2=370;
int Yboton2=220;

void setup() {
  size(1000, 600);
  background(0);

  //cinematica//
  posicionbola= new PVector(Xbola, Ybola);
  velocidadbola= new PVector(5, 5);
  aceleracionbola= new PVector(0.005, 0.005);
}


void draw() {

  background(Rback, 0, Bback); // limpia el fondo

  switch(pantalla) { //comprueba la variable patalla
  case 0:
    menu();
    break;
  case 1:
    nivel1();
    break;
  case 2:
    morir();
    break;
  }
}


void nivel1() {
//acelera la bola y la mueve//
  velocidadbola.add(aceleracionbola);
  posicionbola.add(velocidadbola);

  //bola//
  noStroke();
  fill(255);
  ellipse(posicionbola.x, posicionbola.y, Rb, Rb);

  //pala//
  Xpala=mouseX-50;

  noStroke();
  fill(255);
  rect(Xpala, Ypala, largopala, anchopala);

  //rebote con los lados//
  if ((posicionbola.x>width-Rb)||(posicionbola.x<0) ) {
    velocidadbola.x = velocidadbola.x*(-1);
    aceleracionbola.x = aceleracionbola.x*(-1);
    Rback=Rback+20;
    //   Gback=Gback+20;
    Bback=Bback+20;
  }

  //borde superior//
  if (posicionbola.y<=Rb) {
    velocidadbola.y = velocidadbola.y*(-1);
    aceleracionbola.y = aceleracionbola.y*(-1);
    Rback=Rback+20;
    Bback=Bback+20;
  }
  // borde inferior//
  if (posicionbola.y>=height-Rb) {
    if (vidas==0) {
      pantalla=2;
    } else {
      posicionbola= new PVector(Xbola, Ybola);
      velocidadbola= new PVector(5, 5);
      aceleracionbola= new PVector(0.005, 0.005);
      vidas=vidas-1;
    }
  }
  //texto vidas//
  textSize(30);
  fill(2, 232, 11);
  text("lifes=", 6.7*(width/8), height/9);

  textSize(30);
  fill(2, 232, 11);
  text(vidas, 7.5*(width/8), height/9);

  //texto puntos//
  textSize(30);
  fill(2, 232, 11);
  text("points=", 0.1*(width/8), height/9);

  textSize(30);
  fill(2, 232, 11);
  text(puntos, 1.1*(width/8), height/9);


  //Colores del fondo//
  if (Rback>=255) {
    Rback=0;
  }
  if (Bback>=255) {
    Bback=0;
  }

  //rebote con la pala//
  if ( (posicionbola.x>=Xpala) && (posicionbola.x<=Xpala+largopala) && (posicionbola.y>= (Ypala-(Rb/2))) ) {
    velocidadbola.y = velocidadbola.y*(-1);
    aceleracionbola.y = aceleracionbola.y*(-1);
    puntos=puntos+1;
  } else {
    velocidadbola.y = velocidadbola.y;
    aceleracionbola.y = aceleracionbola.y;
  }
}

void menu() {
  
  //marco del boton//
  noFill();
  stroke(2, 232, 11);
  strokeWeight(10);
  rect(370, 220, largoboton1, anchoboton1);

  //start//
  textSize(75);
  fill(2, 232, 11);
  text("START", width/2 -100, height/2);

  //instrucciones//
  textSize(30);
  fill(2, 232, 11);
  text("control the padle with the mouse", 250, 100);
}

void morir() {
  
  //marco del boton//
  noFill();
  stroke(2, 232, 11);
  strokeWeight(10);
  rect(370, 220, largoboton2, anchoboton2);

  //reinicio//
  textSize(75);
  fill(2, 232, 11);
  text("restart", width/2 -100, height/2);

  //has muerto//
  textSize(50);
  fill(2, 232, 11);
  text("you loose", width/2 -100, 100);
}

void keyPressed () {

  //volver al menú//
  if ( key=='b') {
    pantalla=pantalla-1;
    if (pantalla<0) pantalla =0;
  }
}

void mouseClicked() {

  //Pinchar en el botón de START//
  if ((pantalla == 0)&&(mouseX>=Xboton1)&&(mouseX<=(Xboton1+largoboton1))&&(mouseY>=Yboton1)&&(mouseY<=(Yboton1+anchoboton1))) {
    pantalla=pantalla +1;
    posicionbola= new PVector(Xbola, Ybola);
    velocidadbola= new PVector(5, 5);
    aceleracionbola= new PVector(0.005, 0.005);
    if (pantalla>1) pantalla=0;
  } 
  //pinchar en el boton RESTART//
  if ((pantalla == 2)&&(mouseX>=Xboton2)&&(mouseX<=(Xboton2+largoboton2))&&(mouseY>=Yboton2)&&(mouseY<=(Yboton2+anchoboton2))) {
    pantalla=0;
  }
}
