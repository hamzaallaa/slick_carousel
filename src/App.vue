<template>
  <div id="nav">
       <slick background="#ffffff" :slidesToShow="4" >
         <div class="width_carousel px-2 "> <div class="w-f  d-f ai-c jc-c card "  > Slick </div> </div>
         <div class="width_carousel px-2 "> <div class="w-f  d-f ai-c jc-c card "  > Slick </div> </div>
         <div class="width_carousel px-2 "> <div class="w-f  d-f ai-c jc-c card "  > Slick </div> </div>
         <div class="width_carousel px-2 "> <div class="w-f  d-f ai-c jc-c card "  > Slick </div> </div>
         <div class="width_carousel px-2 "> <div class="w-f  d-f ai-c jc-c card "  > Slick </div> </div>
         <div class="width_carousel px-2 "> <div class="w-f  d-f ai-c jc-c card "  > Slick </div> </div>
       </slick>
  </div>
  <router-view/>
</template>
<script>
// import slicks from './components/slicks.vue'
import slick from './components/slicks-carousel.vue'
export default {
  components: {
    slick
  },
}
</script>
<style lang="scss">
.card{
  height: 100px;
  background: rgb(230, 31, 31);
  border-radius: 5px;
  color: #fff;
}
</style>
