<template>
  <swiper class="swiper">
    <swiper-slide>Slide 1</swiper-slide>
    <swiper-slide>Slide 2</swiper-slide>
    <swiper-slide>Slide 3</swiper-slide>
    <swiper-slide>Slide 4</swiper-slide>
    <swiper-slide>Slide 5</swiper-slide>
    <swiper-slide>Slide 6</swiper-slide>
    <swiper-slide>Slide 7</swiper-slide>
    <swiper-slide>Slide 8</swiper-slide>
    <swiper-slide>Slide 9</swiper-slide>
    <swiper-slide>Slide 10</swiper-slide>
  </swiper>
</template>

<script>
import { Swiper, SwiperSlide } from "vue-awesome-swiper";
import "swiper/css/swiper.css";
import VueAweSomeSwiper from "vue-awesome-swiper";
// eslint-disable-next-line no-undef
Vue.use(VueAweSomeSwiper);

export default {
  name: "swiper-example-default",
  title: "Default",
  components: {
    Swiper,
    SwiperSlide,
  },
};
</script>
