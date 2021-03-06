<template>
  <q-carousel
    ref="slider"
    @slide="__updateCurrentSlide"
    :dots="dots"
    :arrows="arrows"
    :fullscreen="fullscreen"
    :infinite="infinite"
    actions
    :animation="animation"
    :autoplay="autoplay"
    :handle-arrow-keys="handleArrowKeys"
    :easing="easing"
    :swipe-easing="swipeEasing"
    :no-swipe="noSwipe"
    class="text-white bg-black q-gallery-carousel"
    style="height: 300px;"
  >
    <div
      v-for="img in src"
      :key="img"
      slot="slide"
      class="no-padding row flex-center"
    >
      <div class="gallery__image full-width">
        <a :href="img.link">
          <img :src="img.img">
        </a>
      </div>
    </div>

    <div
      class="q-gallery-carousel-overlay"
      :class="{active: quickView}"
      @click="toggleQuickView()"
    ></div>

    <q-icon name="view_carousel" slot="action" @click="toggleQuickView()"></q-icon>

    <div
      class="q-gallery-carousel-quickview"
      :class="{active: quickView, row: horizontalQuickView, horizontal: horizontalQuickView}"
      @touchstart.capture.stop
      @touchmove.capture.stop
      @touchend.capture.stop
      @mousedown.capture.stop
      @mousemove.capture.stop
      @mouseend.capture.stop
    >
      <div v-for="(img, index) in src" :key="img">
        <img
          :src="img.img"
          :class="{active: currentSlide === index}"
          @click="__selectImage(index)"
          style="height: 80px;"
        >
      </div>
    </div>
  </q-carousel>
</template>

<script>
  import {
    QCarousel,
    QIcon
  } from 'quasar'

  export default {
    name: 'q-gallery-carousel',
    components: {
      QCarousel,
      QIcon
    },
    props: {
      dots: Boolean,
      fullscreen: Boolean,
      infinite: Boolean,
      animation: {
        type: [Number, Boolean],
        default: true
      },
      easing: Function,
      swipeEasing: Function,
      noSwipe: Boolean,
      handleArrowKeys: Boolean,
      autoplay: [Number, Boolean],
      src: {
        type: Array,
        required: true
      },
      arrows: {
        type: Boolean,
        default: true
      },
      actions: {
        type: Boolean,
        default: true
      },
      horizontalQuickView: Boolean
    },
    data () {
      return {
        quickView: false,
        currentSlide: 0
      }
    },
    methods: {
      toggleQuickView () {
        this.quickView = !this.quickView
      },
      goToSlide (index, noAnimation) {
        this.$refs.slider.goToSlide(index, noAnimation)
      },
      __selectImage (index) {
        this.goToSlide(index, true)
        this.toggleQuickView()
      },
      __updateCurrentSlide (value) {
        this.currentSlide = value
        this.$emit('slide', value)
      }
    }
  }
</script>

<style>
  .gallery__image {
    object-fit: contain;
  }
</style>
