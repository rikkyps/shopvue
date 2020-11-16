<template>
  <div class="Product">
    <Header/>
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link :to="{name : 'Home'}"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="primaryImage" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel :dots="false" :nav="false" class="product-thumbs-track ps-slider">
                                    <div v-for="image in productDetails.galleries" :key="image.id"
                                    @click.prevent="changePrimaryImage(image.image)" class="pt" :class="image.image ==  primaryImage ? 'active' : ''">
                                        <img :src="image.image" alt="" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>Rp. {{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/shoping-cart">
                                        <a href="#" @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].image)" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->
    <Related/>
    <Footer/>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from '@/components/Header.vue'
import Footer from '../components/Footer.vue'
import Related from '../components/products/Related.vue'

import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
  name: 'Product',
  components: {
    // HelloWorld
    Header,
    Footer,
    carousel,
    Related
  },
  data(){
    return{
      primaryImage: '',
      productDetails: [],
      keranjang: [],
      id: this.$route.params.id
    }
  },
  mounted(){
    if(localStorage.getItem('keranjang')){
        try {
            this.keranjang = JSON.parse(localStorage.getItem('keranjang'))
        } catch(e) {
            localStorage.removeItem('keranjang')
        }
    }

    axios.get('http://shopvue-backend.codehater.net/api/v1/products', {
        params: {id: this.id}
    })
    .then(res => {
        this.getDataImage(res.data.data)
    })
    .catch(err => {
        // eslint-disable-next-line no-console
        console.log(err)
    })
  },
  methods: {
    changePrimaryImage(image){
      this.primaryImage = image
    },
    getDataImage(data){
        this.primaryImage = data.galleries[1].image
        this.productDetails = data
    },
    saveKeranjang(dataId, dataName, dataPrice, dataImage){
        var item = {
            'id': dataId,
            'name': dataName,
            'price': dataPrice,
            'image': dataImage
        }
        this.keranjang.push(item)
        const parsed = JSON.stringify(this.keranjang)
        localStorage.setItem('keranjang', parsed)
    }
  }
}
</script>

<style scoped>
  .product-thumbs .pt {
    margin-right: 10px;
  }
</style>