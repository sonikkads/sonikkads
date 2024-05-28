Paddle leftPaddle, rightPaddle;
Ball ball;
int leftScore = 0;
int rightScore = 0;

void setup() {
  size(800, 600);
  leftPaddle = new Paddle(30);
  rightPaddle = new Paddle(width - 50);
  ball = new Ball();
}

void draw() {
  background(0);
  
  // Draw paddles and ball
  leftPaddle.display();
  rightPaddle.display();
  ball.display();
  
  // Move paddles
  leftPaddle.move();
  rightPaddle.move();
  
  // Move ball
  ball.move();
  ball.checkCollision(leftPaddle, rightPaddle);
  ball.checkScore();
  
  // Display scores
  textSize(32);
  fill(255);
  text(leftScore, width / 4, 50);
  text(rightScore, 3 * width / 4, 50);
}

void keyPressed() {
  if (key == 'w') {
    leftPaddle.up = true;
  } else if (key == 's') {
    leftPaddle.down = true;
  }
  
  if (keyCode == UP) {
    rightPaddle.up = true;
  } else if (keyCode == DOWN) {
    rightPaddle.down = true;
  }
}

void keyReleased() {
  if (key == 'w') {
    leftPaddle.up = false;
  } else if (key == 's') {
    leftPaddle.down = false;
  }
  
  if (keyCode == UP) {
    rightPaddle.up = false;
  } else if (keyCode == DOWN) {
    rightPaddle.down = false;
  }
}

class Paddle {
  float x, y, w, h;
  boolean up, down;
  
  Paddle(float x) {
    this.x = x;
    this.y = height / 2;
    this.w = 20;
    this.h = 100;
    this.up = false;
    this.down = false;
  }
  
  void display() {
    rect(x, y, w, h);
  }
  
  void move() {
    if (up && y > 0) {
      y -= 5;
    }
    if (down && y < height - h) {
      y += 5;
    }
  }
}

class Ball {
  float x, y, r, xSpeed, ySpeed;
  
  Ball() {
    this.x = width / 2;
    this.y = height / 2;
    this.r = 15;
    this.xSpeed = 5;
    this.ySpeed = 5;
  }
  
  void display() {
    ellipse(x, y, r*2, r*2);
  }
  
  void move() {
    x += xSpeed;
    y += ySpeed;
    
    if (y < 0 || y > height) {
      ySpeed *= -1;
    }
  }
  
  void checkCollision(Paddle left, Paddle right) {
    if (x - r < left.x + left.w && y > left.y && y < left.y + left.h) {
      xSpeed *= -1;
    }
    if (x + r > right.x && y > right.y && y < right.y + right.h) {
      xSpeed *= -1;
    }
  }
  
  void checkScore() {
    if (x < 0) {
      rightScore++;
      reset();
    }
    if (x > width) {
      leftScore++;
      reset();
    }
  }
  
  void reset() {
    x = width / 2;
    y = height / 2;
    xSpeed = 5;
    ySpeed = 5;
  }
}
