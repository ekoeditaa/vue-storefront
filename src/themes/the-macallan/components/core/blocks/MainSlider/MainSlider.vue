<template>
  <section class="main-slider w-100 bg-cl-silver cl-white">
    <no-ssr>
      <carousel
        :per-page="1"
        :pagination-enabled="false"
        :navigation-enabled="true"
        :mouse-drag="false"
        navigation-prev-label="<i class='material-icons cl-tertiary'>keyboard_arrow_left</i>"
        navigation-next-label="<i class='material-icons cl-tertiary'>keyboard_arrow_right</i>"
      >
        <slide v-for="(slide, index) in slides" :key="index">
          <div class="background" v-lazy:background-image="slide.image">
            <div class="container px15">
              <div class="row middle-xs">
                <div class="col-md-12">
                  <p class="subtitle mb0 cl-tertiary h4">{{ slide.subtitle }}</p>
                  <h1 class="title mt0 mb30 cl-tertiary">{{ slide.title }}</h1>
                  <div class="inline-flex">
                    <button-outline :link="slide.link" color="dark">
                      {{ slide.button_text }}
                    </button-outline>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </slide>
      </carousel>
    </no-ssr>
  </section>
</template>

<script>
import NoSSR from 'vue-no-ssr'
import { Carousel, Slide } from 'vue-carousel'
import sliderData from 'theme/resource/slider.json'

import MainSlider from 'core/components/blocks/MainSlider/MainSlider'
import ButtonOutline from 'theme/components/theme/ButtonOutline'

export default {
  components: {
    ButtonOutline,
    Carousel,
    Slide,
    'no-ssr': NoSSR
  },
  mixins: [MainSlider],
  created () {
    this.updateSliderData(sliderData)
  }
}
</script>
<style lang="scss">
.main-slider {

  @media (max-width: 767px) {
    display: none;
  }

  .VueCarousel-navigation-prev {
    left: 10px;
  }

  .VueCarousel-navigation-prev {
    right: 10px;
  }

  .VueCarousel-navigation-prev,
  .VueCarousel-navigation-next {
    transform: none!important;
    cursor: pointer;

    i {
      font-size: 42px;
    }

  }

  .VueCarousel-navigation--disabled {
    opacity: .2!important;
  }

}
</style>
<style scoped>
h1 {
  font-size: 72px;
}
.main-slider, .row {
  height: calc(100vh - 54px);
  min-height: 640px;
}
.background {
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
@media (max-width: 75em) {
  .main-slider, .row {
    min-height: 400px;
  }
  .title {
    font-size: 50px;
  }
  .subtitle {
    font-size: 20px;
  }
}
@media (max-width: 64em) {
  .main-slider, .row {
    min-height: 360px;
  }
  .background {
    background-position: left;
  }
  .title {
    font-size: 48px;
  }
  .subtitle {
    font-size: 18px;
  }
  .button {
    font-size: 16px;
  }
}
</style>
