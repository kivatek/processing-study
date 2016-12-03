int unit = 40;
PGraphics pg;
ArrayList<Star> stars = new ArrayList<Star>();

void setup() {
  size(640, 480);
  noStroke();
  noLoop();

  pg = createGraphics(width, height);
  pg.noStroke();

  int wideCount = width / unit;
  int highCount = height / unit;

  for (int y = 0; y < highCount; y++) {
    for (int x = 0; x < wideCount; x++) {
      stars.add(new Star(x * unit, y * unit, 9, 16, 5));
    }
  }
}

void draw() {
  background(0, 0, 12);
  pg.beginDraw();
  pg.ellipse(mouseX, mouseY, 60, 60);
  pg.endDraw();
  image(pg, 0, 0);
  for (Star mod : stars) {
    mod.update();
    mod.display();
  }
}

void mousePressed() {
  //redraw();
  loop();
}

class Star {
  PGraphics bb;
  float x, y;
  float radius1;
  float radius2;
  int npoints;
  float rotation;

  // Contructor
  Star(float xTemp, float yTemp, float radius1Temp, float radius2Temp, int npointsTemp) {
    bb = createGraphics(unit, unit);
    x = xTemp;
    y = yTemp;
    radius1 = radius1Temp;
    radius2 = radius2Temp;
    npoints = npointsTemp;
    rotation = 0.0;
  }

  // Custom method for updating the variables
  void update() {
    rotation += (1.0 / TWO_PI);
  }

  // Custom method for drawing the object
  void display() {
    float angle = TWO_PI / npoints;
    float halfAngle = angle / 2.0;

    bb.beginDraw();
    bb.background(0, 0, 0, 0);
    bb.pushMatrix();
    bb.beginShape();
    bb.fill(255, 216, 0);
    bb.translate(unit / 2, unit / 2);
    bb.rotate(rotation);
    for (float a = 0; a < TWO_PI; a += angle) {
      float sx = sin(a) * radius1;
      float sy = cos(a) * radius1;
      bb.vertex(sx, sy);
      sx = sin(a + halfAngle) * radius2;
      sy = cos(a + halfAngle) * radius2;
      bb.vertex(sx, sy);
    }
    bb.endShape(CLOSE);
    bb.popMatrix();
    bb.endDraw();
    image(bb, x, y);
  }
}
