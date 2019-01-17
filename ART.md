

//link to this sketch/class= https://www.openprocessing.org/sketch/654729
// the p5 libraries= https://p5js.org/libraries/ 

let font,
fontsize = 50;

function preload() 
	{
 		font = loadFont('LOVES.ttf');
	}

var input;
var b;
var a;
var curr;
var prev;
var painting = false;
var fo = 0;
var dir = [];


function setup() 
{
  createCanvas(windowWidth, windowHeight);
	input=createInput('Enter text here + mouse click n be artsy!');
	input.position(650, 100);
	b = createGraphics(1150,1000);
  a = new Ellipse();
	textFont(font);
  textSize(fontsize);
  textAlign(CENTER, CENTER);
		curr = createVector(0,0);
  prev = createVector(0,0);
}

function draw() 
{
	background('black');
	ellipse(mouseX, mouseY, 20, 60);
	b.background('gold');
  b.noFill();
  image(b, 150, 75);
	
  if (millis() > fo && painting) 
		{
			curr.x = mouseX;
    	curr.y = mouseY;
			var force = p5.Vector.sub(curr, prev);
    	force.mult(0.05);
			dir[dir.length - 1].add(curr, force);
    	next = millis() + random(100);
			prev.x = curr.x;
    	prev.y = curr.y;
		}

  
	for( var i = 0; i < dir.length; i++) 
	{
    dir[i].update();
    dir[i].display();
  }
		
	a.grow();
  a.display();
	
	stroke(255);
  strokeWeight(8);
	text(input.value(), windowWidth/2,windowHeight/2);
}



function Ellipse() 
{
  this.x = windowWidth/2;
  this.y = windowHeight/2;
  this.dimension = 1;

  this.grow = function() 
		{
    	this.dimension++;
  	};

  this.display = function() 
	{
  	stroke(204,0,102);
  	strokeWeight(30);
  	ellipseMode(CENTER);
    ellipse(this.x, this.y, this.dimension, this.dimension);
  };
}

function Path() 
{
  this.particles = [];
  this.hue = random(50);
}

Path.prototype.add = function(position, force)
{
 this.particles.push(new Particle(position, force, this.hue));
}


Path.prototype.update = function() 
{  
  for (var i = 0; i < this.particles.length; i++)
		{
    	this.particles[i].update();
  	}
}  


Path.prototype.display = function() 
{
  for (var i = this.particles.length - 1; i >= 0; i--) 
	{
    if (this.particles[i].lifespan <= 0) 
			{
      	this.particles.splice(i, 1);
   		} else 
				{
      		this.particles[i].display(this.particles[i+1]);
    		}
  }

}  

function Particle(position, force, hue) 
{
  this.position = createVector(position.x, position.y);
  this.velocity = createVector(force.x, force.y);
  this.drag = 0.95;
  this.lifespan = 255;
  this.size = 5;
}

Particle.prototype.update = function() 
{
  this.position.add(this.velocity);
  this.velocity.mult(this.drag);
  this.lifespan = this.lifespan-1;
  this.size = this.size+0.05;
}


Particle.prototype.display = function(other)
{
  noStroke();
  fill(0,12, this.lifespan*2);    
  ellipse(this.position.x,this.position.y-(this.size*20), this.size, this.size);    

}

function mousePressed() 
{
	a.dimension = 1;
	redraw();
	fo = 0;
  painting = true;
  prev.x = mouseX;
  prev.y = mouseY;
  dir.push(new Path());
	
}
function mouseReleased()
{
  painting = false;
}
