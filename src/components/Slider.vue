<template>

<div>
  <div id="slider">
    <transition-group tag="div" :name="transitionName" class="slides-group">
      <div v-if="show" :key="current" class="slide">
        <div class="w-75 mx-auto position-relative h-100 d-flex justify-content-center align-items-center py-2">
          <div class="mx-auto h-100 col-8 col-md-6 position-relative">
            <b-img class="slider-image h-100 w-auto" :src="slides[current].main_image"/>
          </div>
          <div class="product-info col-4 col-md-6 text-small h-100 d-flex flex-column justify-content-center align-items-center">
            <h1 @mouseenter="showMore" class="name product-info font-weight-bold text-left text-black d-inline-block pl-0">{{slides[current].name}}</h1>
            <p class="description text-secondary">{{ slides[current].description.substring(0,30) }}...</p>
          </div>
        </div>
      </div>
    </transition-group>
    <div class="btn btn-prev text-white" aria-label="Previous slide" @click="slide(-1)">
      &#10094;
    </div>
    <div class="btn btn-next text-white" aria-label="Next slide" @click="slide(1)">
      &#10095;
    </div>
  </div>

    <modal name="my-first-modal"
                   :width="'85%'"
                   :height="'70%'"
                   :styles="'border-radius: 7px; padding: 25px; display: flex; justify-content: center; align-items: center'">
      <div class="w-100 h-100 mx-auto d-flex justify-content-center">
        <b-img class="h-100 w-auto" :src="currentRandom"/>
      </div>
      <div class="btn btn-next close d-flex text-white font-weight-bold" aria-label="Next slide" @click="showLess">
        X
      </div>
    </modal>

</div>
</template>

<script>

// import  axios from 'axios'

export default {
  name: 'Slider',
  components: {

  },
  data: function () {
   return {
     currentRandom: '',
     sliderInterval: null,
     show: false,
     changedBackround: '',
     modalopen: false,
     current: 0,
     direction: 1,
     transitionName: "fade",
     slides: [
     ]
   }
  },
  async mounted() {
    const url = new URL(
        "https://integrations.yaxint.com/api/products?api_token=70876bc3a88f6644c53af702622edcd8"
    );

    let params = {
      "orderBy": "name",
      "page": "1",
      "limit": "20",
      "product_field_name": "axpol",
      "with_variants": "1",
      "with_categories": "1",
      "with_prints": "1",
      "with_stock": "1",
    };
    Object.keys(params)
        .forEach(key => url.searchParams.append(key, params[key]));

    let headers = {
      "Content-Type": "application/json",
      "Accept": "application/json",
    };

    await fetch(url, {
      method: "GET",
      headers: headers,
    })
        .then(response => response.json())
        .then(json => {
          json.data.forEach(a=>this.slides.push(a))
          this.show = true;
          this.runInterval()
        });

  },

  methods: {
    runInterval () {

      let self = this;
      this.sliderInterval = setInterval(()=> {
        self.slide(1)

      },5000)
    },
    slide(dir) {

      clearInterval(this.sliderInterval)
      this.direction = dir;
      dir === 1
          ? (this.transitionName = "slide-next")
          : (this.transitionName = "slide-prev");
      var len = this.slides.length;
      this.current = (this.current + dir % len + len) % len;
      this.runInterval()

    },

    showMore(){

      this.currentRandom = this.slides[this.current].images[Math.floor(Math.random()*this.slides[this.current].images.length)]
      this.$modal.show('my-first-modal');
      clearInterval(this.sliderInterval)

    },
    showLess(){

      this.$modal.hide('my-first-modal');
      this.runInterval()

    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">


[v-cloak] {
  display: none;
}
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0,0,0,0.5);
  display: table;
  transition: opacity 0.3s ease;
}
.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}
.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}
.modal-body {
  margin: 20px 0;
}
.modal-button {
  float: right;
}
.modal-enter,
.modal-leave {
  opacity: 0;
}
.modal-enter .modal-container,
.modal-leave .modal-container {
  transform: scale(1.05);
}
.close {
  top: 10px !important;
  right: 10px !important;
  background: white;
  opacity: 0.7;
  position: absolute;
  right: 0;
  top: 0;
}
//carousel start
@import url("https://fonts.googleapis.com/css?family=Crimson+Text");

/* FADE IN */
.fade-enter-active {
  transition: opacity 1s;
}
.fade-enter {
  opacity: 0;
}

/* GO TO NEXT SLIDE */
.slide-next-enter-active,
.slide-next-leave-active {
  transition: transform 0.5s ease-in-out;
}
.slide-next-enter {
  transform: translate(100%);
}
.slide-next-leave-to {
  transform: translate(-100%);
}

/* GO TO PREVIOUS SLIDE */
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: transform 0.5s ease-in-out;
}
.slide-prev-enter {
  transform: translate(-100%);
}
.slide-prev-leave-to {
  transform: translate(100%);
}

/* SLIDES CLASSES */

.blue {
  background: #4a69bd;
}

.red {
  background: #e55039;
}

.yellow {
  background: #f6b93b;
}

/* SLIDER STYLES */
body {
  overflow: hidden;
  margin: 0;
  font-size: 50px;
  font-family: "Crimson Text", sans-serif;
  color: #fff;
}

#slider {
  width: 100%;
  height: 45vh;
  position: relative;
}

.slide {
  width: 100%;
  height: 45vh;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn {
  z-index: 10;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  position: absolute;
  top: calc(50% - 20px);
  left: 1%;
  transition: transform 0.3s ease-in-out;
  user-select: none;
  background-color:rgba(0, 0, 0, 0.2);
  color: white;
}

.btn-next {
  left: auto;
  right: 1%;
}

.btn:hover {
  transform: scale(1.1);
}

.slider-image {
  position: absolute;
  right: 0;
}

.product-info {
  z-index: 12;
  padding-left: 10px !important;
  text-align: left !important;
  .name {
    padding-left: 0px !important;
  }
  .description {
    padding-left: 0px !important;
  }
  @media (min-width:320px)  {
    .name {
      font-size: 15px;
    }
    .description {
      font-size: 10px;
    }
  }
  @media (min-width:481px)  {
    .name {
      font-size: 20px;
    }
    .description {
      font-size: 10px;
    }
  }
  @media (min-width:641px)  {
    .name {
      font-size: 25px;
    }
    .description {
      font-size: 15px;
    }
  }
  @media (min-width:961px)  {
    .name {
      font-size: 30px;
    }
    .description {
      font-size: 20px;
    }
  }
  @media (min-width:1025px) {
    .name {
      font-size: 35px;
    }
    .description {
      font-size: 25px;
    }
  }
  @media (min-width:1281px) {
    .name {
      font-size: 40px;
    }
    .description {
      font-size: 30px;
    }
  }

}

//carousel end

</style>
