<template>
  <div>
    <!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :nav="false" :autoplay="true" :dots="false">

                        <div class="product-item" v-for="product in products" :key="product.id">
                            <div class="pi-pic">
                                <img :src="product.galleries[0].image" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a href="#" @click.prevent="saveKeranjang(product.id, product.name, product.price, product.galleries[0].image)"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><router-link :to="{name: 'Product', params: {id:product.id}}">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ product.type }}</div>
                                <a href="#">
                                    <h5>{{ product.name }}</h5>
                                </a>
                                <div class="product-price">
                                    Rp. {{ product.price }}
                                    <span>Rp. 350000</span>
                                </div>
                            </div>
                        </div>

                    </carousel>
                </div>
                <div class="col-lg-12 mt-5" v-else>
                    <p>Data produk terbaru tidak tersedia</p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
  </div>
</template>

<script>
import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
  components: {carousel},
  name: 'slider',
  mounted() {
    if(localStorage.getItem('keranjang')){
        try {
            this.keranjang = JSON.parse(localStorage.getItem('keranjang'))
        } catch(e) {
            localStorage.removeItem('keranjang')
        }
    }

    axios.get('https://shopvue-backend.codehater.net/api/v1/products')
        .then(res => {
            this.products = res.data.data.data
        })
        .catch(err => {
            console.log(err.response)
        })

  },
  data() {
      return {
          products: [],
          keranjang: []
      }
  },
  methods: {
      saveKeranjang(id, name, price, image){
          let data = {
              'id': id,
              'name': name,
              'price': price,
              'image': image
          }

          this.keranjang.push(data)
          const parsed = JSON.stringify(this.keranjang)
          localStorage.setItem('keranjang', parsed)

          window.location.reload()
      }
  }
}
</script>

<style scoped>
  .product-item{
    margin-right: 25px;
  }
</style>