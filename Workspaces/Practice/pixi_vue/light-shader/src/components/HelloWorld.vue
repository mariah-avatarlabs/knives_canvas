<template>
  <div>
      <canvas 
        :id="'canvas' + (index - 1)"
        width="900" 
        height="150"
        v-for="index in 10" 
        :key="index"        
      >
        An alternative text describing what your canvas displays. 
      </canvas>

      <!-- <canvas 
        id="canvas" 
        width="900" 
        height="150"
        v-for="index in 10" 
        :key="index"        
      >
        An alternative text describing what your canvas displays. 
      </canvas> -->
  </div>
</template>

<script>
import imageclip from '../assets/knife_overlay.png'
// import TWEEN from '@tweenjs/tween.js';


export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      last_known_scroll_position: 0,
      ticking: false,
      imgPosY: 0,
      cans: [],
      rootCan: null,
      rootContext: null
    }
  },
  mounted() {


    var canvasElArray = document.getElementsByTagName("canvas");

    for (let cans in canvasElArray){

      var canID = "canvas" + cans;
        this.loadImage(canID);

    }

    this.assignListners();


  },
  methods: {
    loadImage(canvasID){
        // var img = new Image();
        var currCan = document.getElementById(canvasID);

        if(currCan !== null && currCan){
          currCan.posY = 0;
          var ctx = currCan.getContext('2d');

          // currCan.imgClip = new Image;
          // currCan.imgClip.onload = () => {
          //   this.drawImageProp(ctx, currCan.imgClip)
          //   ctx.globalCompositeOperation = 'source-in';
          // }
          // currCan.imgClip.src = imageclip;


          currCan.mainImg = new Image();
          currCan.mainImg.onload = function() { 
              ctx.drawImage(currCan.mainImg, 0, 0);
          };
          currCan.mainImg.src = 'https://vastphotos.com/files/uploads/photos/10310/large-format-photo-print-of-mountains-and-lakes-m.jpg';
        }

    },

    animateCans(){
      var canvasElArray = document.getElementsByTagName("canvas");

      for (let cans in canvasElArray){
        var canID = "canvas" + cans;
        this.animateOnScroll(canID);
      }

    },

    animateOnScroll(canvasID) {

      var currCan = document.getElementById(canvasID);

      if(currCan !== null && currCan){
        var ctx = currCan.getContext('2d');

        ctx.clearRect(0, 0, innerWidth, innerHeight);

        // this.drawImageProp(ctx, currCan.imgClip)
        // ctx.globalCompositeOperation = 'source-in';
        ctx.drawImage(currCan.mainImg, 0, currCan.posY);

        currCan.posY -= 10
      }



    },    
    assignListners() {      
      window.addEventListener('scroll', (e) => {
            this.animateCans();

          // if (!this.isOutOfViewport(this.rootCan)){
          //   this.animateCans();
          // }

      });


    },
    animate() {
      this.rootContext.clearRect(0, 0, innerWidth, innerHeight);
      this.rootContext.drawImage(this.rootCan.img, 0, this.imgPosY);

      this.imgPosY -= 600

      requestAnimationFrame(this.animate);
    },

    isOutOfViewport(elem) {

      // Get element's bounding
      var bounding = elem.getBoundingClientRect();

      // Check if it's out of the viewport on each side
      var out = {};
      out.top = bounding.top < 0;
      // out.left = bounding.left < 0;
      // out.bottom = bounding.bottom > (window.innerHeight || document.documentElement.clientHeight);
      // out.right = bounding.right > (window.innerWidth || document.documentElement.clientWidth);
      // out.any = out.top || out.left || out.bottom || out.right;
      // out.all = out.top && out.left && out.bottom && out.right;

      console.log(out.top);
      return out.top;

    },

    drawImageProp(ctx, img, x, y, w, h, offsetX, offsetY) {

      if (arguments.length === 2) {
          x = y = 0;
          w = ctx.canvas.width;
          h = ctx.canvas.height;
      }

      // default offset is center
      offsetX = typeof offsetX === "number" ? offsetX : 0.5;
      offsetY = typeof offsetY === "number" ? offsetY : 0.5;

      // keep bounds [0.0, 1.0]
      if (offsetX < 0) offsetX = 0;
      if (offsetY < 0) offsetY = 0;
      if (offsetX > 1) offsetX = 1;
      if (offsetY > 1) offsetY = 1;

      var iw = img.width,
          ih = img.height,
          r = Math.min(w / iw, h / ih),
          nw = iw * r,   // new prop. width
          nh = ih * r,   // new prop. height
          cx, cy, cw, ch, ar = 1;

      // decide which gap to fill    
      if (nw < w) ar = w / nw;                             
      if (Math.abs(ar - 1) < 1e-14 && nh < h) ar = h / nh;  // updated
      nw *= ar;
      nh *= ar;

      // calc source rectangle
      cw = iw / (nw / w);
      ch = ih / (nh / h);

      cx = (iw - cw) * offsetX;
      cy = (ih - ch) * offsetY;

      // make sure source rectangle is valid
      if (cx < 0) cx = 0;
      if (cy < 0) cy = 0;
      if (cw > iw) cw = iw;
      if (ch > ih) ch = ih;

      // fill image in dest. rectangle
      ctx.drawImage(img, cx, cy, cw, ch,  x, y, w, h);
    },




  },

  created(){
  },


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
html, body {
  scroll-behavior: smooth;
}

canvas {
  width: 100%;
  margin-bottom: 50%;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
