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
	dta(){
		return {
			pixiapp: null,
		}
	},
	created(){
		let that = this;
	},
	mounted(){
		let that = this;

		class ParticleText {

			constructor(){
				that.pixiapp = new PIXI.Application();
				that.$refs.appp.appendChild(that.pixiapp.view);
				that.pixiapp.renderer.resize(1920,1080);
				this.addObjects();
				console.log(that.pixiapp);
				console.log(that.$refs.appp);
			}
			addObjects(){

				PIXI.loader.add('textt', 'text.png').load((loader, resources) => {

					let textt = new PIXI.Sprite(resources.textt.texture);
					textt.x = 0;
					textt.y = that.pixiapp.renderer.height / 2;
					textt.anchor.x = 0;
					textt.anchor.y = 0.5;

					that.pixiapp.stage.addChild(textt);

					this.animate();

				});

			}

			animate(){
				that.pixiapp.ticker.add(() => {

				});
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
