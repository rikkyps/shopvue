<template>
  <div class="Shopping">
    <Header/>

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link :to="{name: 'Home'}"><i class="fa fa-home"></i> Home</router-link>
                        <span>Shopping Cart</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody v-if="keranjang.length > 0">
                                        <tr v-for="keranjang in keranjang" :key="keranjang.id">
                                            <td class="cart-pic first-row">
                                                <img class="image-cart" :src="keranjang.image" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5>{{ keranjang.name }}</h5>
                                            </td>
                                            <td class="p-price first-row">Rp. {{ keranjang.price }}</td>
                                            <td class="delete-item"><a href="#" @click="removeItem(keranjang.id)"><i class="material-icons">
                                              close
                                              </i></a></td>
                                        </tr>
                                    </tbody>
                                    <tbody v-else>
                                        <tr>
                                            <td colspan="4">Keranjang Kosong</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8">
                            <h4 class="mb-4 text-left">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout text-left">
                                <form >
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input v-model="customerInfo.name" type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama">
                                        <small class="text-danger" v-if="error.name">{{ error.name }}</small>
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input type="email" v-model="customerInfo.email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email">
                                        <small class="text-danger" v-if="error.email">{{ error.email }}</small>
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input type="number" v-model="customerInfo.phone" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP">
                                        <small class="text-danger" v-if="error.phone">{{ error.phone }}</small>
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea v-model="customerInfo.address" class="form-control" id="alamatLengkap" rows="3"></textarea>
                                        <small class="text-danger" v-if="error.address">{{ error.address }}</small>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout text-left">
                                <ul>
                                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3">Subtotal <span>Rp. {{ subTotal }}</span></li>
                                    <li class="subtotal mt-3">Pajak <span>10%</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>Rp. {{ grandTotal }}</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                </ul>
                                <a class="proceed-btn" href="#" @click.prevent="checkout">I ALREADY PAID</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Shopping Cart Section End -->

  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from '@/components/Header.vue'
import axios from 'axios'

export default {
  name: 'Shopping',
  components: {
    Header
  },
  data(){
      return{
          keranjang: [],
          customerInfo: {
              name: null,
              email: null,
              phone: null,
              address: null
          },
          error: ''
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
  },
  computed: {
      subTotal(){
          let n = 0
          for(let i = 0; i < this.keranjang.length; i++){
              n += parseInt(this.keranjang[i].price)
          }
          return n
      },
      grandTotal(){
          let n = 0
          n = this.subTotal + (this.subTotal * 10) / 100
          return n
      }
  },
  methods: {
    removeItem(idx) {
        let keranjangStorage = JSON.parse(localStorage.getItem('keranjang'))
        let itemKeranjangStorage = keranjangStorage.map(itemKeranjangStorage => itemKeranjangStorage.id)
        let index = itemKeranjangStorage.findIndex(id => id == idx)
        this.keranjang.splice(index, 1)
        const parsed = JSON.stringify(this.keranjang)
        localStorage.setItem('keranjang', parsed)
    },

    checkout(){

        if(this.grandTotal == 0){
            alert('Masukan produk terlebih dahulu')
        } else {
        let productIds = this.keranjang.map(function(product){
            return product.id
        })

        let checkoutData = {
            'name': this.customerInfo.name,
            'email': this.customerInfo.email,
            'phone': this.customerInfo.phone,
            'address': this.customerInfo.address,
            'grandTotal': this.grandTotal,
            'status': 'PENDING',
            'transaction_details': productIds
        }

        axios.post('https://www.shopvue-backend.codehater.net/api/v1/checkout', checkoutData)
            .then(res => {
                this.$router.push({name: 'Success'})
                // eslint-disable-next-line no-console
                console.log(res.data)
            })  
            .catch(err => {
                // eslint-disable-next-line no-console
                console.log(err)
                this.error = err.response.data.errors
            })
        }
        
    }
  }

}
</script>

<style scoped>
    .image-cart{
        width: 80px;
        height: 80px;
    }
</style>
