<template>
  <div class="page-wrapper">
    <p class="mouse">{{mousePos}}</p>
    <div class="scattered-images">
      <figure v-for="image, index in images" :key="index" @mouseup="nowId=-1" @mousedown="selectId($event,index)">
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
      allFigures: [],
    }
  },
  watch:{
		mousePos(){
			// console.log(this.mousePos.x, this.mousePos.y)
			if (this.nowId != -1){
        console.log('this on', this.allFigures[this.nowId]);
				this.allFigures[this.nowId].style.left = this.mousePos.x - this.startMousePos.x
        this.allFigures[this.nowId].style.top = this.mousePos.y- this.startMousePos.y
			}
		}
	},
  methods: {
		// imageCss (i) {
    //   // console.log('i', i);
		// 	return {
		// 		left: i.x,
		// 		top: i.y,
		// 	}
		// },
		selectId(evt, id){
			this.nowId = id
			this.startMousePos = {
				x: evt.offsetX,
				y: evt.offsetY
			}
      console.log('now ID', this.nowId);
			console.log('where mouse', this.startMousePos.x,this.startMousePos.y)
		},
  
    scatterImages() {
      this.allFigures = document.querySelectorAll('figure')
      console.log('scattering...');
      const gridWidth = window.innerWidth / 6
      const gridHeight = window.innerHeight / 6
      let currentFigure = 0
      // for each row...
      for (let i = 0; i < window.innerHeight / gridHeight; i++) {
        // for each column
        for(let j = 0; j < window.innerWidth / gridWidth; j++) {
          // until we run out of images
          if(currentFigure < this.allFigures.length) {
            // console.log('x, y', gridWidth * j, gridHeight * i)
            // add position to current image
            this.allFigures[currentFigure].style.top = gridHeight * i+ 'px'
            this.allFigures[currentFigure].style.left = gridWidth * j + 'px'
            // create random size and rotation variables
            const randomSize = 1 + Math.random()
            const randomRotation = Math.random() * 360
            // apply those to the img tag inside the current image
            this.allFigures[currentFigure].childNodes[0].style.transform = `scale(${randomSize}) rotate(${randomRotation}deg)`
            currentFigure++
          } else {
            return
          }
        }
      }
      this.allFigures.forEach((image, index) => {
     
      })
    }
  },
  mounted() {
    console.log('I have mounted')
    this.scatterImages()
    window.onmousemove = (evt) => {
      this.mousePos = {
        x: evt.pageX,
        y: evt.pageY
      }
    }

    window.onmouseup = (evt) => {
      this.nowId = -1
    }
  },


}
</script>

<style scoped>
body {
  @apply p-0;
}

h1 {
  @apply pb-4;
  font-size: clamp(1rem, 10vw, 10rem);
}

.scattered-images {
  @apply grid grid-cols-2 sm:grid-cols-4 md:grid-cols-6 lg:grid-cols-8 gap-4;
}

.active {
  background-color: yellow;
}

span.thing {
  text-decoration: underline;
}

figure {
  position: absolute;
  max-width: 200px;
  @apply p-4;
}

figure img {
  transition: border .2s;
  border: 5px solid transparent;
}

figure  {
  mix-blend-mode: difference;
  cursor: grab;
}

figure:hover img{
  border: 5px solid black;
}

.modal {
  position: fixed;
  background-color: gray;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border: 10px solid red;
  display: grid;
  place-content: center;
}


.modal .close-button {
  position: fixed;
  top: 2rem;
  right: 2rem;
  font-size: 4rem;
  line-height: 1;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.mouse {
  position: fixed;
  color: red;
  z-index: 999;
}
</style>