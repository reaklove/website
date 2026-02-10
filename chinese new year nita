<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chinese New Year Animation</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            overflow: hidden;
            background: linear-gradient(to bottom, #0a0a1a 0%, #1a0a0a 100%);
            font-family: 'Arial', sans-serif;
        }

        canvas {
            display: block;
        }

        .greeting {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            color: #FFD700;
            font-size: 3em;
            font-weight: bold;
            text-shadow: 0 0 20px #ff0000, 0 0 40px #ff0000;
            animation: pulse 2s ease-in-out infinite;
            pointer-events: none;
            z-index: 10;
        }

        .greeting .subtitle {
            font-size: 0.4em;
            margin-top: 10px;
            color: #ff6b6b;
        }

        @keyframes pulse {
            0%, 100% {
                transform: translate(-50%, -50%) scale(1);
                opacity: 1;
            }
            50% {
                transform: translate(-50%, -50%) scale(1.05);
                opacity: 0.9;
            }
        }
    </style>
</head>
<body>
    <canvas id="canvas"></canvas>
    <div class="greeting">
        恭喜发财
        <div class="subtitle">Happy Chinese New year "nita"!</div>
    </div>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });

        // Firework class
        class Firework {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.particles = [];
                this.exploded = false;
                this.vy = -8 - Math.random() * 4;
                this.gravity = 0.1;
                this.color = `hsl(${Math.random() * 60 + (Math.random() > 0.5 ? 0 : 340)}, 100%, 60%)`;
                
                // Create explosion particles
                const particleCount = 80 + Math.random() * 40;
                for (let i = 0; i < particleCount; i++) {
                    const angle = (Math.PI * 2 * i) / particleCount;
                    const speed = 2 + Math.random() * 4;
                    this.particles.push({
                        x: this.x,
                        y: this.y,
                        vx: Math.cos(angle) * speed,
                        vy: Math.sin(angle) * speed,
                        life: 1,
                        decay: 0.01 + Math.random() * 0.01
                    });
                }
            }

            update() {
                if (!this.exploded) {
                    this.vy += this.gravity;
                    this.y += this.vy;
                    
                    if (this.vy >= 0) {
                        this.exploded = true;
                    }
                } else {
                    this.particles.forEach(p => {
                        p.x += p.vx;
                        p.y += p.vy;
                        p.vy += 0.05;
                        p.life -= p.decay;
                    });
                    this.particles = this.particles.filter(p => p.life > 0);
                }
            }

            draw() {
                if (!this.exploded) {
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, 3, 0, Math.PI * 2);
                    ctx.fillStyle = this.color;
                    ctx.fill();
                    
                    // Trail
                    ctx.beginPath();
                    ctx.moveTo(this.x, this.y);
                    ctx.lineTo(this.x, this.y - this.vy * 3);
                    ctx.strokeStyle = this.color;
                    ctx.lineWidth = 2;
                    ctx.stroke();
                } else {
                    this.particles.forEach(p => {
                        ctx.beginPath();
                        ctx.arc(p.x, p.y, 2, 0, Math.PI * 2);
                        ctx.fillStyle = this.color;
                        ctx.globalAlpha = p.life;
                        ctx.fill();
                        ctx.globalAlpha = 1;
                    });
                }
            }

            isDead() {
                return this.exploded && this.particles.length === 0;
            }
        }

        // Lantern class
        class Lantern {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = -50;
                this.width = 30 + Math.random() * 20;
                this.height = this.width * 1.4;
                this.speed = 0.5 + Math.random() * 0.5;
                this.swing = 0;
                this.swingSpeed = 0.02 + Math.random() * 0.02;
                this.color = Math.random() > 0.5 ? '#ff0000' : '#FFD700';
            }

            update() {
                this.y += this.speed;
                this.swing += this.swingSpeed;
            }

            draw() {
                ctx.save();
                ctx.translate(this.x + Math.sin(this.swing) * 10, this.y);
                
                // Lantern body
                ctx.fillStyle = this.color;
                ctx.shadowColor = this.color;
                ctx.shadowBlur = 20;
                
                // Top cap
                ctx.fillRect(-this.width/2, -5, this.width, 5);
                
                // Main body
                ctx.beginPath();
                ctx.ellipse(0, this.height/2, this.width/2, this.height/2, 0, 0, Math.PI * 2);
                ctx.fill();
                
                // Bottom tassel
                ctx.fillStyle = '#FFD700';
                ctx.fillRect(-2, this.height, 4, 15);
                
                ctx.shadowBlur = 0;
                ctx.restore();
            }

            isOffScreen() {
                return this.y > canvas.height + 50;
            }
        }

        // Particle system
        let fireworks = [];
        let lanterns = [];

        // Create initial lanterns
        for (let i = 0; i < 8; i++) {
            const lantern = new Lantern();
            lantern.y = Math.random() * canvas.height;
            lanterns.push(lantern);
        }

        // Animation loop
        function animate() {
            ctx.fillStyle = 'rgba(10, 10, 26, 0.1)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // Add new fireworks randomly
            if (Math.random() < 0.03) {
                const x = canvas.width * 0.2 + Math.random() * canvas.width * 0.6;
                fireworks.push(new Firework(x, canvas.height));
            }

            // Add new lanterns
            if (Math.random() < 0.01) {
                lanterns.push(new Lantern());
            }

            // Update and draw lanterns
            lanterns.forEach((lantern, index) => {
                lantern.update();
                lantern.draw();
                if (lantern.isOffScreen()) {
                    lanterns.splice(index, 1);
                }
            });

            // Update and draw fireworks
            fireworks.forEach((firework, index) => {
                firework.update();
                firework.draw();
                if (firework.isDead()) {
                    fireworks.splice(index, 1);
                }
            });

            requestAnimationFrame(animate);
        }

        // Click to add firework
        canvas.addEventListener('click', (e) => {
            fireworks.push(new Firework(e.clientX, canvas.height));
        });

        animate();
    </script>
</body>
</html>
