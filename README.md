# Asteroids
# A game I've been learning to make and progress in Java.

//#rectm8
//int position.y, position.x; //SHIP1
//int position.y2, position.x2; //SHIP2
Boolean move;
Boolean up = false, left = false, right = false;
Boolean fire = false;

PImage ship;
PImage bg;
PImage ring;
PImage enemy;


PVector position = new PVector(); 
PVector velocity = new PVector();
PVector acceleration = new PVector();
PVector accelerator = new PVector();

float rotation;
float topSpeed = 10;
float slowDown = .95;
//store only bullets
ArrayList <Bullet> bullets = new ArrayList();

ArrayList <Asteroid> asteroids = new ArrayList();


//_____________________

//SETTINGS
void setup() {
  size (1280, 720);
  frameRate(60);
  move = false;
  position.x = width/2;
  position.y = height/2;
  velocity.set(0, 0);
  acceleration.set(0, 0);
  accelerator.set(0, -0.3);
  rotation = 0;
  ship = loadImage("sanic.png");
  bg = loadImage("Sonic1.jpg");
  ring = loadImage("FINALRING.png");
  enemy = loadImage("enemy.png");

  for (int i = 0; i < 10; i++) {
    Asteroid a = new Asteroid();
    asteroids.add(a);
  }
}
//FUNCTION TO DISPLAY
void draw() {
  removeToLimit(15);//some other code that removes bullets if there are too many on screen
  background(bg);
  testKeys();
  pushMatrix();
  imageMode(CENTER);

  move();
  drawBullets();
  drawAsteroids();
  translate(position.x, position.y);

  drawSpaceship();
  popMatrix();

  keeponscreen();
}

void removeToLimit(int maxLength)
{
  while (bullets.size () > maxLength)
  {
    bullets.remove(0);
  }
}

void drawBullets() {
  //    start  ;      check        ; interate (i = i + 1)
  for (int i = 0; i < bullets.size (); i++) {
    println(i);
    Bullet b = bullets.get(i);
    b.draw();
  }
}

void drawAsteroids() {
  for ( int i = 0; i < asteroids.size (); i++) {
    Asteroid a = asteroids.get(i);
    a.draw();
  }
}

//CONTROLS
void testKeys() {
  if (up) {
    acceleration.add(accelerator);
  }
  if (left) {
    rotation = rotation - radians(7);
    accelerator.rotate(radians(-7));
  }
  if (right) {
    rotation = rotation + radians(7);
    accelerator.rotate(radians(7));
  }
  if (fire) {
    Bullet b = new Bullet(rotation, position.get());
    bullets.add(b);
    println("Bullet size : " + bullets.size());
  }
}



//MOVE
void move() {
  //acceleration.add(accelerator);
  velocity.add(acceleration);
  velocity.limit(topSpeed);
  position.add(velocity);
  velocity.y = slowDown * velocity.y;
  velocity.x = slowDown * velocity.x;
}


//KEYS
void keyPressed() {
  acceleration.set(0, 0);

  //  println(key);
  //  println(keyCode);
  //position.x
  if (keyCode == 39) {
    right = true;
    //position.x = position.x + 11;
  } else if (keyCode == 37) {
    left = true;
    //position.x = position.x - 11;
  }

  if (keyCode == 40) {
    velocity.y = velocity.y * .6;
    velocity.x = velocity.x * .6;
    //    acceleration.sub(accelerator);
    //        acceleration.y = acceleration.y + .1;
    //    position.y = position.y + 11;
  } else if (keyCode == 38) {
    up = true;
    velocity.y = velocity.y - .01;
    //    position.y = position.y - 11;
  }
  if (keyCode == 32);
  {
    fire = true;
  }
}


//SPACESHIP
void drawSpaceship() {
  rotate(rotation);
  image(ship, 0, 0, 200, 200);
}


//STAYINSIDE
void keeponscreen() {
  if (position.x > width) {
    position.x = 0;
  } else if ( position.x < 0 ) { 
    position.x = width;
  }
  if (position.y > height) {
    position.y = 0;
  } else if ( position.y < 0 ) { 
    position.y = height;
  }
}
void keyReleased() {
  if (keyCode == 39) {
    right = false;
  }
  if (keyCode == 37) {
    left = false;
  }
  if (keyCode == 38) {
    up = false;
  }
  if (keyCode == 32) {
    fire = false;
  }


  acceleration.set(0, 0);
}

/////////////

class Asteroid {

  float size;
  PVector position;
  PVector velocity;


  Asteroid() {
    this.position = new PVector();
    this.position.set((random(0, width)), (random(0, height)));
    this.velocity = new PVector();
    this.velocity.set(random(-1, 2), random(-1, 2));

    size = random(60, 60);
  }

  void draw() {
    this.position.add(velocity);
    this.keepOnscreen();
    image(enemy, this.position.x, this.position.y, size, size);
  }


void keepOnscreen() {
  if (this.position.x > width) {
    this.position.x = 0;
  } else if ( this.position.x < 0 ) { 
    this.position.x = width;
  }
  if (this.position.y > height) {
    this.position.y = 0;
  } else if ( this.position.y < 0 ) { 
    this.position.y = height;
  }
}
} 
////////////////
class Bullet {
  PVector position;
  float rotation;
  PVector velocity;

  Bullet(float r, PVector p) {
    this.rotation = r; 
    this.position = p;

    this.velocity = new PVector();
    this.velocity.set(0, 25);
    this.velocity.rotate(this.rotation + random(radians(10)) + PI);
  }
  void draw() {
    this.position.add(this.velocity);
    pushMatrix();
    //translate(-this.position.x, -this.position.y);
    //rotate(this.rotation);
    //translate(this.position.x, this.position.y);
    image(ring, position.x, position.y, 40, 40);
    popMatrix();
  }

}




