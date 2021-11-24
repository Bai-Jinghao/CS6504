# CS6504
Agile lab Exerice
Shape s[]=new Shape[100];

class Shape {
  final int SEGMENT = 200;
  final float NOISE_SCALE = 2;
  ArrayList<PVector> points;
  ArrayList<PVector> chars;
  float x, y, r;
  float startAngle;
  PFont f;
  color c;
//to setup
  Shape(float _x, float _y, float _r, color c_) {//change timeable 2 v
    x = _x;
    y = _y;
    r = _r; 
    c=c_;

    points = new ArrayList();
    for (int i = 0; i < SEGMENT; i++) {
      float theta = map(i, 0, SEGMENT - 1, 0, TAU);
      points.add(new PVector(theta, 0));
    }
  }
    

float rByTheta(float theta) {
    float time = float(frameCount) / 100;
    float scale = noise(cos(theta) + 1, sin(theta) + 1, time) * NOISE_SCALE + 1;
    return  r * scale ;
  }

float angleByTheta(float theta) {
    final float offset = 0.06;
    float theta1 = theta - offset, theta2 = theta + offset;
    float a = rByTheta(theta1);
    float b = rByTheta(theta2);
    return atan2(a * sin(theta1) - b * sin(theta2), a * cos(theta1) - b * cos(theta2)) + PI;
  }//color

void rotateText() {
    startAngle = atan2(mouseY - y, mouseX - x);
  }
void update() {
    for (PVector p : points) {
      p.y = rByTheta(p.x);
  
    }
  }

void display() {
    pushMatrix();
    translate(x+mouseX-width/2, y+mouseY-height/2);
    beginShape();
    fill(c);
    noStroke();
    for (PVector p : points) {
      vertex(p.y * cos(p.x), p.y * sin(p.x));
    }
    endShape();
    fill(255, 0, 0);
    popMatrix();
  }
}//how big of circle
void setup() {
  size(1000, 800);
  noStroke();
  colorMode(HSB, 255);
  for (int i=0; i<100; i++) {
    color color1=color(200-i*2, 155, 255, i+10);
    fill( color1);
    rotate(3*i/PI);
    s[i] = new Shape(width / 2, height /2, 120-i, color1);
  }
}  

void draw() {
  background(0);
  noCursor();
  fill(0,20);
  int r;
  r=int(random(20,100));//change the shape
  for(int i=0;i<r;i++)  
  {
    
    s[i].update();
    delay(10);
    s[i].update();
    delay(10);
    s[i].display();
    delay(10);
    s[i].update();
  }
   
}
