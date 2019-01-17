# ART's markdown- a series.
## by Kanika Mehta

**Description:**

My sketch is an interactive and fun class which allows the user to input text of their choice, create a champagne fizz effect by mouse click and movement and watch the expansion of an ellipse which can be restarted at the click of your mouse. The inspiration for my coding was derived from https://www.openprocessing.org/sketch/654728 sketch on OpenProcessing and from http://p5js.site44.com/002/sketch.js by Purin Phanichphant. 





**Methods:**

- function preload()

This function ensures that my font 'LOVES' in the ttf format is pre-loaded by the time I want it executed in my sketch. While running my sketch, be sure to save it first and then upload 'LOVES.ttf' from the files section on the right hand side.

**BE SURE TO DOWNLOAD THE FONT 'LOVES' FROM https://www.dafont.com/loves.font**

*click on the download button next to the font -> go to Files section present on the right side of my sketch -> click on Upload files -> open the 'loves' folder which you downloaded and select LOVES.ttf* . This font is now loaded. 



- function setup()

Our canvas of window width and height is created via the **createCanvas** function.
For additional interactivity, I added the **createInput** function which creates a text input field on the page. This element is a box where the user can type in text of choice. 

*b = createGraphics(1150,1000);* - Variable b is the object of our **createGraphics** function. This function creates and returns a new p5.Renderer object [b]. This function is used to draw into an off-screen graphics buffer. The two parameters define the width and height in pixels.

*a = new Ellipse();* - Variable a is our object for the function ellipse which is discussed later down.

*textFont(font);* *textSize(fontsize);* *textAlign(CENTER, CENTER);* - The font for our text [ our pre-loaded font- LOVES.ttf ], text size [50] and text allignment [ center of the window ] are set.

*curr = createVector(0,0);*
*prev = createVector(0,0);* Vectors are created for curr which holds the 'where we are now' position of our champagne fizz particles and for prev which holds the 'where we were before' position of our champagne fizz particles. 


- function draw()

*background('black');* - Background of our canvas set to black.

*ellipse(mouseX, mouseY, 20, 60);* - Our cursor which is the shape of an ellipse has its x and y coordinates set to the continuously changing x and y positions depending on where annd how we move our mouse. 20 and 60 are the width and height of our ellipse respectively. 

*b.background('gold');
  b.noFill();
 image(b, 150, 75);* - This is the designing of the off screen graphics buffer. [ the gold coloured screen ]
 
 *if (millis() > fo && painting) { ... }*  
 
   1. If it is time to produce a new point/particle on the screen, we grab the current position of our mouse. [  *curr.x = mouseX;
    curr.y = mouseY;* ]
    
   2.  The force of the particle is based on the mouse's movement.[ *var force = p5.Vector.sub(curr, prev);
    force.mult(0.05);* ]
 
   3.  New particle is added. (current position + force) [ *dir[dir.length - 1].add(curr, force);* ]

   4.  The next particle is scheduled. [ *next = millis() + random(100);* ]

   5.  The current mouse values are stored in the previous coordinates. [  *prev.x = curr.x;
    prev.y = curr.y;* ]
    
    
*for( var i = 0; i < dir.length; i++) {
    dir[i].update();
    dir[i].display();
  }* -    The trail/path of the particles is updated and drawn/ displayed. 
  
  *a.grow();
  a.display();* -  grow and display are the calling functions of ellipse where the expansion and display of the ellipse are respectively executed.
  
  *stroke(255);
  	strokeWeight(8);
	text(input.value(), windowWidth/2,windowHeight/2);* -  The text's design is coded. input.value() means that the text entered in the text input field/ textbox by the user will be displayed on the screen as well. 
  
  
- function Ellipse()

*this.x = windowWidth/2;
  this.y = windowHeight/2;* - this.x and this.y are refering to the x and y dimensions for the origin of our ellipse (the center of the ellipse) which is the center of our window. [current objects]
  
 *this.dimension = 1;* - Our ellipse's dimension is initially set to 1 [ minute size ]. 
 
 *this.grow = function() {
    this.dimension++;
  };* - With the gradual growth in the size of the ellipse, the dimensions increase accordingly. 
  
  *this.display = function() {
  	stroke(204,0,102);
  	strokeWeight(30);
  	ellipseMode(CENTER);*
    - The design of the ellipse is coded. 
    
  *ellipse(this.x, this.y, this.dimension, this.dimension);* - The x and y coordinates of the center of the ellipse is the window's center. this.dimension is the gradually increasing width and height of our expanding ellipse.
  

- function Path()

This function creates a trail/ list of the particles.

- *Path.prototype.add = function(position, force){
 this.particles.push(new Particle(position, force, this.hue));
}*   - This adds position, force and hue to a new particle. 

- *Path.prototype.update = function() {  
  for (var i = 0; i < this.particles.length; i++) {
    this.particles[i].update();
  }
}* - This method is to update the positions of the particles generated.

- *Path.prototype.display = function() {....}* - This method is coded to display the path or trail of particles generated by the click and movement of the mouse. The trail doesnt remain on the screen permanently. The initally drawn particles' trail begins to fade out out first followed by the latter bits of the trail. '.splice' is used to remove one particle's position at a time from the trail. 

- function Particle(position, force, hue) 

This function is dedicated to the creating a life span ( how long are the particles supposed to last on the screen ), size and drag rate of the particles with the position varying with the movement of the mouse and velocity of the particles at different x and y positions stored respectively. 

- Particle.prototype.update = function() {
  this.position.add(this.velocity);
  this.velocity.mult(this.drag);
  this.lifespan = this.lifespan-1;
  this.size = this.size+0.05;
}

 This is to move the particles around. 
 
 *this.position.add(this.velocity);* - slows the particle down.
 *this.velocity.mult(this.drag);* - fades the particle out. 
*this.lifespan = this.lifespan-1;
  this.size = this.size+0.05;* - The particles' lifespan and size expansion while gradual fading are coded. 
  
  - Particle.prototype.display = function(other) {
  noStroke();
  fill(0,12, this.lifespan*2);    
  ellipse(this.position.x,this.position.y-(this.size*20), this.size, this.size);    
}

This draws the particles and connects them with imanginary lines. One line is connected to the other and so on to create the path. 



- function mousePressed() 

When the mouth is pressed, the ellipse is created with a dimension 1 [ starting from its initial minute size] . redraw() causes the draw to be executed once every time the mouse is pressed. We begin painting the trail of particle on the screen. The mouse's coordinates begin to start getting stored in the 'where we were before' variables [ prev] as we draw the trail. 


- function mouseReleased()

As soon as the mouse is released, the trail begins to gradually disappear. No painting of the trail is done. The ellipse expansion is not interrupted. 



This is the markdown for my sketch ART which is available on OpenProcessing. Do check it out!


Thanks,

Kanika Mehta





 
 
 
 


  
  
  
  
 
 
 

  
   
 
 

 
 








