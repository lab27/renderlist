<template>
  <div class="page-wrapper">
    <h1>{{title}}</h1>
    <div class="grid-list">
      <figure v-for="image, index in images" :key="index" :class="image.size">
        <img :src="`/img/${image.path}`" alt="">
        <!-- <p>{{image.size}}, {{image.hasChildren}}</p> -->
      </figure>
    </div>
  </div>
</template>

<script>
const images = require('~/assets/data/images.json')
export default {
  data() {
    return {
      title: 'My things',
      images: images,
      color: '',
      items: [ //array!
        {id: '001', type: 'car', color: 'red'}, //object!
        {id: '002', type: 'umbrella', color: 'pink'},
        {id: '003', type: 'broom', color: 'brown'},
        {id: '004', type: 'counter', color: 'grey'},
        {id: '005', type: 'table', color: 'yellow'},
      ] 
    }
  },
  created() {
    console.log('I have been created')
  },
  beforeMount() {
    console.log('I am about to mount')
  },
  mounted() {
    console.log('I have mounted')
  },


}
</script>

<style>
body {
  @apply p-12;
}

h1 {
  @apply pb-4;
  font-size: clamp(1rem, 10vw, 10rem);
}

.grid-list {
  @apply grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-12;
}

.active {
  background-color: yellow;
}

span.thing {
  text-decoration: underline;
}

figure.small {
  @apply col-span-2;
  border: 10px solid salmon;
  width:100%;
  
}

figure.medium {
  @apply col-span-4;
  border: 10px solid dodgerblue;
  width:100%;
}

figure.large {
  @apply col-span-6;
  border: 10px solid lime;
  width:100%;
}
</style>