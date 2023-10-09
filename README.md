    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Particle Animation</title>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: rgba(0, 0, 0, 1);
            filter: blur(3px) contrast(10);
        }
        #ui-controls {
            display: none;
            position: fixed;
            top: 10px;
            left: 10px;
            background-color: rgba(255, 255, 255, 0.8);
            padding: 10px;
            border-radius: 5px;
        }
        #canvas 
        {
background: black;
position: absolute;
top:0;
left:0;



        }

        #canvas1 {
            
    display: none;
    
 
}
    </style>
    </head>
    <body>
    <canvas id="canvas1"></canvas>
    <div id="ui-controls">
        <label>
            Particle Count:
            <input type="range" id="particle-count" min="0" max="1000" value="300">
        </label>
        <br>
        <label>
            Particle Size:
            <input type="range" id="particle-size" min="1" max="20" value="10">
        </label>
        <br>
        <label>
            Connection Distance:
            <input type="range" id="connection-distance" min="0" max="200" value="80">
        </label>
        <br>
        <label>
            Connections:
            <input type="checkbox" id="connections-toggle" checked>
        </label>
    </div>
    <script>


        const canvas = document.getElementById('canvas1');
        const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;


        const mainCanvas = document.createElement('canvas');
mainCanvas.width = window.innerWidth;
mainCanvas.height = window.innerHeight;
document.body.appendChild(mainCanvas);
let mainContext = mainCanvas.getContext('2d');
mainContext.save();
let mover = 0;

let arrayofcolors = [
    'white', 'gold', 'blue', 'green', 
    'red', 'purple', 'orange', 'pink',
    'brown', 'gray', 'cyan', 'magenta',
    '#FF6347', '#90EE90', '#FF4500', '#2E8B57',
    'rgb(255, 105, 180)', 'rgb(240, 230, 140)', 'rgb(70, 130, 180)', 'rgb(152, 251, 152)'
];
        let gradient = ctx.createLinearGradient(0,0,canvas.width, canvas.height);
        gradient.addColorStop(0, arrayofcolors[Math.floor(Math.random()*19.88)]);
        gradient.addColorStop(0.5, arrayofcolors[Math.floor(Math.random()*19.88)]);
        gradient.addColorStop(1, arrayofcolors[Math.floor(Math.random()*19.88)]);
        ctx.fillStyle = gradient;
        ctx.strokeStyle = 'white';


        let a = Math.random();
let b=Math.random();
let c = Math.random()*1000;
let c1 = Math.random()*1000;
let rotation0 = Math.floor(Math.random() * 1.5);
let rotation1 = Math.floor(Math.random() * 1.5);
let direct = Math.floor(Math.random() * 1.99);

        function reset() {
            let arrayofcolors = [
    'white', 'gold', 'blue', 'green', 
    'red', 'purple', 'orange', 'pink',
    'brown', 'gray', 'cyan', 'magenta',
    '#FF6347', '#90EE90', '#FF4500', '#2E8B57',
    'rgb(255, 105, 180)', 'rgb(240, 230, 140)', 'rgb(70, 130, 180)', 'rgb(152, 251, 152)'
];
    // Clear both canvases
    ctx.clearRect(0, 0, ctx.width, ctx.height);
    mainContext.clearRect(0, 0, mainCanvas.width, mainCanvas.height);
    a = Math.random();
b=Math.random();
 c = Math.random()*1000;
c1 = Math.random()*1000;
ctx.restore();
mainContext.restore();
mainContext.fillRect(0,0,1000,1000);
let gradient1 = ctx.createLinearGradient(0,0,canvas.width, canvas.height);
        gradient1.addColorStop(0, arrayofcolors[Math.floor(Math.random()*19.88)]);
        gradient1.addColorStop(0.5, arrayofcolors[Math.floor(Math.random()*19.88)]);
        gradient1.addColorStop(1, arrayofcolors[Math.floor(Math.random()*19.88)]);
        ctx.fillStyle = gradient1;
 rotation0 = Math.floor(Math.random() * 1.5);
 rotation1 = Math.floor(Math.random() * 1.5);
// Generate random values between 0 and 20
let randomBlur = Math.floor(Math.random() * 21);
let randomContrast = Math.floor(Math.random() * 21);
direct = Math.floor(Math.random() * 1.99);
// Create the filter style string
let filterStyle = `filter: blur(${randomBlur}px) contrast(${randomContrast});background: black;`;
console.log(filterStyle)
// Now you can apply this string as a style to an HTML element
document.body.style = filterStyle;
    // Re-initialize your effect object or other necessary variables
    effect = new Effect(ctx, ctx);
}



        class Particle {
        constructor(effect, size) {
            this.effect = effect;
            this.radius = size;
            this.x = this.radius + Math.random() * (this.effect.width - this.radius * 2);
            this.y = this.radius + Math.random() * (this.effect.height - this.radius * 2);
            this.vx = Math.random() * 1 - 0.5;
            this.vy = Math.random() * 1 - 0.5;
            this.pushX = 0;
            this.pushY = -1;
            this.friction = 0.99;
        }
        
        draw(context) {
            context.beginPath();
            context.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
            context.fill();
        }
        
        update() {
            if (this.effect.mouse.pressed) {
                const dx = this.x - this.effect.mouse.x;
                const dy = this.y - this.effect.mouse.y;
                const distance = Math.hypot(dx, dy);
                const force = (this.effect.mouse.radius / distance);
                if (distance < this.effect.mouse.radius) {
                    const angle = Math.atan2(dy, dx);
                    this.pushX += Math.cos(angle) * force;
                    this.pushY += Math.sin(angle) * force;
                }
            }

            this.x += (this.pushX *= this.friction) + this.vx;
            this.y += (this.pushY *= this.friction) + this.vy;

            if (this.x < this.radius) {
                this.x = this.radius;
                this.vx *= -1;
            } else if (this.x > this.effect.width - this.radius) {
                this.x = this.effect.width - this.radius;
                this.vx *= -1;
            }
            if (this.y < this.radius) {
                this.y = this.radius;
                this.vy *= -1;
            } else if (this.y > this.effect.height - this.radius) {
                this.y = this.effect.height - this.radius;
                this.vy *= -1;
            }
        }
        
        reset() {
            this.x = this.radius + Math.random() * (this.effect.width - this.radius * 2);
            this.y = this.radius + Math.random() * (this.effect.height - this.radius * 2);
        }
    }       

    class Effect {
        constructor(canvas, context) {
        this.canvas = canvas;
        this.context = context;
        this.width = this.canvas.width;
        this.height = this.canvas.height;
        this.particles = [];
        this.numberOfParticles = Math.floor(Math.random()*100);
        this.createParticles(Math.floor(Math.random()*30),this.numberOfParticles);  // Assuming the initial particle size is 10
        this.connectionDistance = 100;
        this.showConnections = true;
        
        this.mouse = {
            x: 0,
            y: 0,
            pressed: false,
            radius: 200
        }

        window.addEventListener('resize', e => {
            this.resize(e.target.innerWidth, e.target.innerHeight);
        });

        window.addEventListener('mousemove', e => {
            if (this.mouse.pressed) {
                this.mouse.x = e.x;
                this.mouse.y = e.y;
            }
        });

        window.addEventListener('mousedown', e => {
            this.mouse.pressed = true;
            this.mouse.x = e.x;
            this.mouse.y = e.y;
        });

        window.addEventListener('mouseup', e => {
            this.mouse.pressed = false;
        });
    }

        

    createParticles(size, count) {
    if (!size || !count || count < 0 || size < 0) {
        console.error('Invalid arguments:', size, count);
        return;
    }
    console.log(size, count);  // Add this line

    // Update the size and randomize the velocity of existing particles
    this.particles.forEach(particle => {
        particle.radius = size;
        particle.vx = Math.random() * 1 - 0.5;
        particle.vy = Math.random() * 1 - 0.5;
    });

    // Create new particles or update existing ones
    for (let i = 0; i < count; i++) {
        if (this.particles[i]) {
            // Update existing particle
            this.particles[i].radius = size;
        } else {
            // Create new particle
            this.particles.push(new Particle(this, size));
        }
    }

    // Remove excess particles
    while (this.particles.length > count) {
        this.particles.pop();
    }
}

        
        handleParticles(context) {
            this.connectParticles(context);
            this.particles.forEach(particle => {
                particle.draw(context);
                particle.update();
            });
        }

        connectParticles(context) {
    if (!this.showConnections) return;
    const maxDistance = this.connectionDistance;
    for (let a = 0; a < this.particles.length; a++) {
        for (let b = a + 1; b < this.particles.length; b++) {
            const dx = this.particles[a].x - this.particles[b].x;
            const dy = this.particles[a].y - this.particles[b].y;
            const distance = Math.hypot(dx, dy);
            
            // Connection logic
            if (distance < maxDistance) {
                context.save();
                const opacity = 1 - (distance / maxDistance);
                context.globalAlpha = opacity;
                context.strokeStyle = "blue";
                context.beginPath();
                context.moveTo(this.particles[a].x, this.particles[a].y);
                context.lineTo(this.particles[b].x, this.particles[b].y);
                context.stroke();
                context.restore();


            // Collision detection logic
            const combinedRadii = this.particles[a].radius + this.particles[b].radius;
            if (distance < combinedRadii) {
                this.resolveCollision(this.particles[a], this.particles[b]);
            }
            }
            

        }
    }
}

 resolveCollision(particleA, particleB) {
    // Determine the vector between the centers of the two particles
    const dx = particleA.x - particleB.x;
    const dy = particleA.y - particleB.y;

    // Determine the distance between the particles
    const distance = Math.sqrt(dx*dx + dy*dy);
    
    // Avoid further calculation if the particles are overlapping
    if (distance === 0) return;

    // Calculate the unit normal and unit tangent vectors
    const unx = dx / distance;
    const uny = dy / distance;
    const utx = -uny;
    const uty = unx;

    // Separate the velocities into components normal and tangent to the collision
    const v1n = particleA.vx * unx + particleA.vy * uny;
    const v1t = particleA.vx * utx + particleA.vy * uty;
    const v2n = particleB.vx * unx + particleB.vy * uny;
    const v2t = particleB.vx * utx + particleB.vy * uty;

    // Assume equal masses (m1 = m2 = 1) for simplicity
    // For unequal masses, you would use the equations of an elastic collision
    const m1 = 1;
    const m2 = 1;

    // Compute the new normal velocities based on an elastic collision
    const v1nPrime = ((m1 - m2) * v1n + 2 * m2 * v2n) / (m1 + m2);
    const v2nPrime = ((m2 - m1) * v2n + 2 * m1 * v1n) / (m1 + m2);

    // Convert the scalar normal and tangent velocities back to vector form
    particleA.vx = v1nPrime * unx + v1t * utx;
    particleA.vy = v1nPrime * uny + v1t * uty;
    particleB.vx = v2nPrime * unx + v2t * utx;
    particleB.vy = v2nPrime * uny + v2t * uty;
}

        resize(width, height) {
            this.canvas.width = width;
            this.canvas.height = height;
            this.width = width;
            this.height = height;
            const gradient = this.context.createLinearGradient(0, 0, width, height);
            gradient.addColorStop(0, 'white');
            gradient.addColorStop(0.5, 'gold');
            gradient.addColorStop(1, 'orangered');
            this.context.fillStyle = gradient;
            this.context.strokeStyle = 'white';
            this.particles.forEach(particle => {
                particle.reset();
            });
        }
    }
    ctx.save();
    
    setInterval(reset, 30000);
        let effect = new Effect(canvas, ctx);

        document.getElementById('particle-count').addEventListener('input', (event) => {
    const count = event.target.value;
    const size = document.getElementById('particle-size').value;
    effect.createParticles(size, count);
    
});

document.getElementById('particle-size').addEventListener('input', (event) => {
    const size = event.target.value;
    const count = effect.numberOfParticles;  // get the current number of particles
    effect.createParticles(size, count);     // pass both size and count
});

        document.getElementById('connection-distance').addEventListener('input', (event) => {
            effect.connectionDistance = event.target.value;
        });

        document.getElementById('connections-toggle').addEventListener('change', (event) => {
            effect.showConnections = event.target.checked;
        });
let direction = 1;

        function animate(){
            //ctx.clearRect(0, 0, canvas.width, canvas.height);
            effect.handleParticles(ctx);
            mover = mover+direction;
           
          // Scan move
            // if (mover+500>canvas.width){
            //     mover = mover=2;
            // }

                        if (mover+500>canvas.width||mover<0){
               direction=- direction;
               effect.resize(innerWidth, innerHeight);
            }
            if (rotation1!=0)
            {
            ctx.rotate(a/10);
            }
            if (direct==0) {
                mainContext.drawImage(canvas, mover, 0,500,500);
            mainContext.drawImage(canvas, canvas.width-50,10*Math.sin(mover),50, canvas.height,500+mover, 0,5,500);
            mainContext.drawImage(canvas, canvas.width-50,10*Math.sin(mover),50, canvas.height,0+mover, 500,5,500);
            mainContext.drawImage(canvas, canvas.width-250,10*Math.sin(mover),50, canvas.height,500+mover, 500,5,500);
            }
            else {
                mainContext.drawImage(canvas, 0, mover,500,500);
            mainContext.drawImage(canvas, 10*Math.sin(mover),canvas.height-50,canvas.width, 50,0, 500+mover,500,5);
            mainContext.drawImage(canvas, 10*Math.sin(mover),canvas.height-50,canvas.width, 50,500, 0+mover,500,5);
            mainContext.drawImage(canvas, 10*Math.sin(mover),canvas.height-250,canvas.width, 50,0, 500+mover,500,5);
         

            }
           if (rotation0!=0)
           {
           
            mainContext.translate(c,c1)
            mainContext.rotate(b)
            mainContext.translate(-c,-c1)
           }
            requestAnimationFrame(animate);
        }
        animate();
    </script>
    </body>
    </html>
