<template>
	<div id="app" ref="appp">
	</div>
</template>

<script>

import * as PIXI from 'pixi.js'

export default {
	name: 'app',
	components: {
	},
	data(){
		return {
		}
	},
	created(){
		
	},
	mounted(){
				let that = this;
		

		class ParticleText {

			constructor(){
				this.pixiapp = new PIXI.Application(window.innerWidth, window.innerHeight, {
					resolution: window.devicePixelRatio,
					autoresize: true
				});
				that.$refs.appp.appendChild(this.pixiapp.view);
				this.particleSize = 2;
				this.rows = 60;
				this.cols = 800;
				this.particles = [];
				this.container = new PIXI.ParticleContainer(15000);
				console.log(this.container);
				this.pixiapp.stage.addChild(this.container);
				this.addObjects();
			}
			addObjects(){

				PIXI.loader.add('textt', 'text.png').load((loader, resources) => {

					const canvas = document.createElement('canvas');
					const ctx = canvas.getContext('2d');
					canvas.width = this.cols*this.particleSize;
					canvas.height = this.rows*this.particleSize;
					ctx.drawImage(resources.textt.data, 0, 0);

					function hasFill(x,y, size) {
						for (let i = 0; i < size; i+=1) {
							for (let j = 0; j < size; j+=1) {
								if(ctx.getImageData(x+i, y+i,1,1).data[2]>0) return true;
							}
						}
						return false;
					}


					for (let i = 0; i < this.cols; i+=1) {
						for (let j = 0; j < this.rows; j+=1) {
							if(hasFill(i*this.particleSize, j*this.particleSize, this.particleSize)) {
								const p = new Particle(i*this.particleSize, j*this.particleSize, resources.textt.texture, this.particleSize);
								this.particles.push(p);
								this.container.addChild(p.sprite);
							}
						}
					}

					this.animate();

				});

			}

			animate(){

				this.pixiapp.ticker.add(() => {

					this.mouse = this.pixiapp.renderer.plugins.interaction.mouse.global;
					this.particles.forEach(p => {
						p.update(this.mouse);
					});

				});

			}
		};

		class Particle {
			constructor(x,y, texture, size){

				this.x = x;
				this.y = y;
				this.sprite = new PIXI.Sprite(new PIXI.Texture(texture));
				this.sprite.texture.frame = new PIXI.Rectangle(x,y,size,size);
				this.sprite.x = x;
				this.sprite.y = y;
				this.speedX = 0;
				this.speedY = 0;
				this.radius = 100;
				this.friction = 0.9;
				this.gravity = 0.01;
				this.maxGravity = 0.01 + Math.random()*0.03
				this.dirX = Math.random() - 0.5;
				this.dirY = Math.random() - 0.5;

			}
			update(mouse){

				const distanceX = mouse.x - this.sprite.x;
				const distanceY = mouse.y - this.sprite.y;

				const distance = Math.sqrt(distanceX**2 + distanceY**2);

				const normalX = distanceX/distance;
				const normalY = distanceY/distance;

				if(distance < this.radius) {
					this.gravity *= this.friction;
					this.speedX -= normalX;
					this.speedY -= normalY;
				} else{
					this.gravity += 0.1*(this.maxGravity - this.gravity);
				}

				const oDistX = this.x - this.sprite.x;
				const oDistY = this.y - this.sprite.y;

				this.speedX += oDistX*this.gravity;
				this.speedY += oDistY*this.gravity;


				this.speedX *= this.friction;
				this.speedY *= this.friction;

				this.sprite.x += this.speedX;
				this.sprite.y += this.speedY;

			}

		};

		let PT = new ParticleText();

	},
}
</script>

<style lang="sass">
	#app
		text-align: center
		color: #2c3e50
		background-color: white
		width: 100vw
		height: 100vh
		position: absolute
		top: 0
		left: 0
</style>
