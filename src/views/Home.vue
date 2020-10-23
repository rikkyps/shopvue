<template>
  <div class="home">
    <Header/>
    <HeroCarousel/>
    <Slider/>
    <Instagram/>
    <Partner/>
    <Footer/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from '@/components/Header.vue'
import HeroCarousel from '@/components/HeroCarousel.vue'
import Slider from '@/components/Slider.vue'
import Instagram from '@/components/Instagram.vue'
import Partner from '../components/Partner.vue'
import Footer from '../components/Footer.vue'

export default {
  name: 'Home',
  components: {
    // HelloWorld
    Header,
    HeroCarousel,
    Slider,
    Instagram,
    Partner,
    Footer
  }
}
</script>
