<template>
   <div :id="randomId" class="wapper-slick w-f" >
      <div class=" mov p-r o-h" :style="{'background':background}" @mousedown="mousedown($event)" @mouseleave="mouseLive()" @mouseup="mouseup()"  @mousemove="mouseover($event)"   @touchend="mouseup('mobile')" @touchstart="mousedown($event.touches[0])" @touchmove="mouseover($event.touches[0])" >
        <div class="o-h p-r c-p " :class="[(!over?'tr-1/2':''),(contentCenter?'m-auto':'')]" :style="[direction=='ltr'?{'left':this.leftMov+'px'}:{'right':this.leftMov+'px'},{'width':widthContentCard+'%'}]"  >
          <div class="d-f ai-c " >
            <div class="py-3 d-f ai-c slide_width" :style="{'width':widthOneArray+'%'}" v-if="!hidefarstLastArry" >
              <slot ></slot>
            </div>
            <div class="py-3 d-f ai-c slide_width" :style="{'width':widthOneArray+'%'}" >
              <slot ></slot>
            </div>
            <div  class="py-3 d-f ai-c slide_width" :style="{'width':widthOneArray+'%'}" v-if="!hidefarstLastArry" >
              <slot ></slot>
            </div>
          </div>
        </div>
      <div class=" zi-999 p-a w-f h-f l-0 r-0 b-0 t-0 d-n" :class="{'d-b':showModel}"></div>
        <svg xmlns="http://www.w3.org/2000/svg" @click="prev()" class="h-6 w-6 my-auto zi-999 b-t p-a l-0 v-h o-0 prev t-1/3 d-f ai-c" :class="{'v-v o-1':play}" fill="none" viewBox="0 0 24 24"  stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round"   stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
        <svg xmlns="http://www.w3.org/2000/svg" @click="next()" class="h-6 w-6 my-auto zi-999 b-t p-a r-0 v-h o-0 next t-1/3 d-f ai-c" :class="{'v-v o-1':play}" fill="none" viewBox="0 0 24 24" stroke="currentColor">
           <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
        </svg>
     </div>
  </div>
</template>

<script>
export default {
   props:{
     slidesToShow: { type: Number,default:4 }, //number of items in screen
     speed:{type:Number ,default:2000}, //Movement speed
     autoPlay:{type:Boolean,default:true}, //stop start play moving
     direction:{type:String,default:'ltr'}, //stop start play moving
     ratio:{type:String,default:"1:1"},
      maxHeight:{type:[Number,String],default:300},
     background:{type:String,default:"#e6e3e3aa"},
    fit:{type:String,default:'cover'},
    shadow:{type:Boolean,default:false}, //box shadow cader image,
    contentCenter:{type:Boolean,default:false}
   },
   data() {
        return {
            over:false,
            position:null,
            leftMov:0,
            showModel:false,
            lastPosition:0,
            play:false,
            widthCard:null,
            sizeArrayImage:0,
            cleartimeout:null,
            widthContentCard:null,
            numberMov:null,
            hidefarstLastArry:false,
            widthOneArray:null,
            randomId:'_'+Math.floor(Math.random()*10000),
        }
   },
   async mounted() {
    this.randomId='_'+Math.floor(Math.random()*10000)
   this.normale()
},
    methods: {
   normale(){
    //number of items in screen
      var content=document.querySelectorAll('#'+this.randomId+' .width_carousel')
      console.log('====>',content)
      if(content.length==0){
       window.timeOut =  setTimeout(()=>{
          this.normale()
       },2000)
      }else{
         clearTimeout(window.timeOut);
        var numberItem=content.length
        this.widthCard=(100/(numberItem/3))
        content.forEach(ele=>{
          ele.style.width=this.widthCard+'%'
        })
        this.widthContentCard=(100/this.slidesToShow)*numberItem
        if((numberItem/3)>this.slidesToShow)this.widthOneArray=this.widthContentCard/3
        else {
          this.widthContentCard=this.widthContentCard/3
          this.widthOneArray=100
          this.hidefarstLastArry=true
        }
         var  card=document.querySelector('.wapper-slick')
        // //Item moving number
            setTimeout(()=>{
              content.forEach(ele=>{
                var item=window.getComputedStyle(ele).width
                this.numberMov=item.substring(0,item.length-2)
                this.sizeArrayImage+=parseInt(this.numberMov)
            })
                this.sizeArrayImage=this.sizeArrayImage/3
           //    Stop moving  all elements when they  are smaller than screen width
            if(this.sizeArrayImage>card.offsetWidth) {
              this.leftMov=-this.sizeArrayImage
                this.playMoving()
            }
        },1000)
       }
   },
   next(){

      this.leftMov += parseInt(this.numberMov)
      if(this.leftMov<-this.sizeArrayImage*2)this.leftMov=-this.sizeArrayImage
      if(this.leftMov>-this.sizeArrayImage)this.leftMov=-this.sizeArrayImage*2
   },
   prev(){
      if(this.leftMov<-this.sizeArrayImage*2)this.leftMov=-this.sizeArrayImage
      if(this.leftMov>-this.sizeArrayImage)this.leftMov=-this.sizeArrayImage*2
           this.leftMov-=parseInt(this.numberMov)
   },
    mousedown(e){
     if(!this.hidefarstLastArry){
       this.over=true
       this.position=e.clientX
     }
     },
     mouseup(target){
      if(!this.hidefarstLastArry){
        this.showModel=false
        if(this.leftMov>0){
          this.leftMov=0
          this.lastPosition=0
        }
        else{
          this.lastPosition=this.leftMov
        }
        this.over=false
      }
      if(target=="mobile"){
          this.mouseLive()
      }
    },
     mouseLive(){
      if(!this.hidefarstLastArry){
        this.showModel=false
          clearTimeout(this.cleartimeout)
             this.play=false
           this.cleartimeout=setTimeout(()=>{
             this.playMoving()
           },this.speed)
        if(this.leftMov>0){
          this.leftMov=0
          this.lastPosition=0
        }
        else{
          this.lastPosition=this.leftMov
        }
        this.over=false
      }
     },
    mouseover(e){
       if(!this.hidefarstLastArry){
         this.play=true
         if(this.over){
           this.showModel=true
           this.leftMov=this.lastPosition+(e.clientX-this.position)
           if(this.leftMov<-this.sizeArrayImage*2)this.leftMov=-this.sizeArrayImage
            if(this.leftMov>-this.sizeArrayImage)this.leftMov=-this.sizeArrayImage*2
         }
       }
    },
     playMoving(){
      if(this.autoPlay) {
        if(!this.hidefarstLastArry){
          if(!this.play){
           this.leftMov-=parseInt(this.numberMov)
            this.cleartimeout= setTimeout(()=>{
              this.playMoving()
                this.over=false
              if(this.leftMov<-this.sizeArrayImage*2){
                this.over=true
                this.leftMov=-this.sizeArrayImage
                // this.leftMov-=parseInt(this.numberMov)
              }
              if(this.leftMov>-this.sizeArrayImage)this.leftMov=-this.sizeArrayImage*2
           },this.speed)
          }
        }
      }
   }
 },
}
</script>

<style>

</style>