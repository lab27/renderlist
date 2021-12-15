<template>
  <div class="page-wrapper">
    <p class="mouse">{{mousePos}}</p>
    <div class="scattered-images">
      <figure v-for="image, index in images" :key="index" :style="imageCss(image)" @mouseup="nowId=-1" @mousedown="selectId($event,index)">
        <img :src="`/img/${image.path}`" alt="">
      </figure>
    </div>
  </div>
</template>

<script>

const newImages = require('~/assets/data/newImages.json')
export default {

  data() {
    return {
      nowId: -1,
      mousePos: {
        x: 0,
        y: 0
      },
      startMousePos: {
        x: 0,
        y: 0
      },
      images: newImages,
    }
  },
	watch:{
		mousePos(){
			// console.log(this.mousePos.x, this.mousePos.y)
			if (this.nowId != -1){
				this.images[this.nowId].x = this.mousePos.x - this.startMousePos.x
				this.images[this.nowId].y = this.mousePos.y - this.startMousePos.y
				
			}
		}
	},
  methods: {
		imageCss (i) {
			return {
				left: i.x +'px',
				top: i.y +'px',
			}
		},
		selectId(evt, id){
      console.log('Mouse down!!');
			this.nowId = id
			this.startMousePos = {
				x: evt.offsetX,
				y: evt.offsetY
			}
      console.log('now ID', this.nowId);
			console.log('where mouse', this.startMousePos.x,this.startMousePos.y)
		},
  
    scatterImages() {
      console.log('scattering...');
      const allFigures = document.querySelectorAll('figure')
      allFigures.forEach(figure => {
        // set some random size and rotation vars
        const randomSize = 1 + Math.random()
        const randomRotation = Math.random() * 360
        // apply those to the img tag inside the current image
        figure.childNodes[0].style.transform = `scale(${randomSize}) rotate(${randomRotation}deg)`
      })
    }
  },
  beforeMount() {
      // calculate grid sizes based on window
      const gridWidth = window.innerWidth / 6
      const gridHeight = window.innerHeight / 6
      let currentFigure = 0
      // Set x and y properties on all images:
      // for each row...
      for (let i = 0; i < window.innerHeight / gridHeight; i++) {
        // for each column
        for(let j = 0; j < window.innerWidth / gridWidth; j++) {
          // until we run out of images
          if(currentFigure < this.images.length) {
            // add position to current image
            this.$set(this.images[currentFigure], 'x', Math.floor(gridWidth * j))
            this.$set(this.images[currentFigure], 'y', Math.floor(gridHeight * i))
            currentFigure++
          } 
        }
      }
  },
  mounted() {
    this.scatterImages()

    window.onmousemove = (evt) => {
      this.mousePos = {
        x: evt.pageX,
        y: evt.pageY
      }
    }
  },

}
</script>

<style>
html {
  overflow: hidden;
}

body {
  @apply p-0;
  overflow: hidden;
}

figure {
  position: absolute;
  max-width: 200px;
  @apply p-4;
}

figure img {
  transition: border .2s;
  border: 5px solid transparent;
  pointer-events: none;
}

figure  {
  mix-blend-mode: difference;
  cursor: grab;
  user-select: none;
}

  figure:hover {
    z-index: 999;
  }
figure:hover img{
  border: 5px solid black;
}





.mouse {
  position: fixed;
  color: red;
  z-index: 999;
}
</style>