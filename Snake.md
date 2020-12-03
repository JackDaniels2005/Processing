# Processing
int snakeBodySize = 25;
int speed = 50;
boolean GameState = true;

// Snake Variables
int dir = 0; // 0 right, 1 down, 2 left, 3 up
int[][] s; // Array that keeps count of body position  ----> s[i][0] = x value   &&   s[i][1] = y value
int[][] ts; // Array used to be able to expand the Array s
int snakeSize = 1;

// Fruit Variables
int fx;
int fy;

void setup() {
  s = new int[1][2]; // Snake body is 1 large
  ts = s;
  size(1000, 1000);
  frameRate(100);
  noStroke();
  s[0][0] = 5;  // Grid based map ---> CENTER VALUE OF X IS 5
  s[0][1] = 5;  // Grid based map ---> CENTER VALUE OF Y IS 5  
  fruit();
}

void draw() {
  if (GameState == false) {
    background(0);
    textSize(50);
    strokeWeight(5);
    stroke(#FF9933);
    fill(0);
    rect(width/2-100, height/2-50, 200, 100);
    fill(#FF9933);
    textMode(CENTER);
    text("PLAY", width/2-75, height/2+10);
  }
  noStroke();
  if ( GameState ) {
    if (keyPressed == true) {
      switch(key) {
      case 'd':
        if (dir !=2) {
          dir = 0;
        }
        break;
      case 's':
        if (dir !=3) {
          dir = 1;
        }
        break;
      case 'a':
        if (dir !=0) {  
          dir = 2;
        }
        break;
      case 'w':
        if (dir !=1) {
          dir = 3;
        }
        break;
      case 'D':
        if (dir !=2) {
          dir = 0;
        }

        break;
      case 'S':
        if (dir !=3) {
          dir = 1;
        }
        break;
      case 'A':
        if (dir !=0) {
          dir = 2;
        }
        break;
      case 'W':
        if (dir !=1) {
          dir = 3;
        }
        break;
      }
    }

    if (frameCount%speed == 0) {
      background(0);
      update();
      display();    

      for (int i = 0; i<snakeSize; i++) {
        if (s[i][0] <0 || s[i][0]>width/snakeBodySize || s[i][1] <0 || s[i][1]>height/snakeBodySize) {
          frameRate(0);
          background(255);
        }
        for (int j = 0; j<snakeSize; j++) {
          if (i!=j) {
            if (s[i][0] == s[j][0] && s[i][1] == s[j][1]) {
              frameRate(0);
              background(255);
            }
          }
        }
      }
    }
  }
}

void update() {
  ts = s;
  if (s[0][0] == fx && s[0][1] == fy) {
    snakeSize++;
    fruit();
    speed = speed-2;
    if (speed <=0) {
      speed = 2;
    }
  }
  s = new int[snakeSize][2];

  for (int i = snakeSize-1; i>0; i--) {
    s[i][0] = ts[i-1][0];
    s[i][1] = ts[i-1][1];
  }

  switch(dir) {
  case 0:
    s[0][0] = ts[0][0] +1;   // snake part 0 moves to the right
    s[0][1] = ts[0][1];
    break;
  case 1:
    s[0][1]  = ts[0][1] +1;  // snake part 0 moves to the bottomn
    s[0][0] = ts[0][0];
    break;
  case 2:
    s[0][0]  = ts[0][0] -1;   // snake part 0 moves to the left
    s[0][1] = ts[0][1];
    break;
  case 3:
    s[0][1]  = ts[0][1] -1;   // snake part 0 moves to the right
    s[0][0] = ts[0][0];
    break;
  }
}

void fruit() {
  fx = floor(random(0, (width/snakeBodySize)));
  fy = floor(random(0, (height/snakeBodySize)));
}

void display() {  // Function that displays the enterity of the snake && the fruit
  fill(255, 0, 0);
  rect(fx *snakeBodySize, fy *snakeBodySize, snakeBodySize, snakeBodySize);
  fill(0, 255, 0);

  for (int i = 0; i < snakeSize; i++) {
    rect(s[i][0]*snakeBodySize, s[i][1]*snakeBodySize, snakeBodySize, snakeBodySize);
  }
  fill(20);
  rect(s[0][0]*snakeBodySize+5, s[0][1]*snakeBodySize+5, 5, 5);
}
