<template>
  <div>
    <!-- Header Section Begin -->
    <header class="header-section">
        <div class="header-top">
            <div class="container">
                <div class="ht-left">
                    <div class="mail-service">
                        <i class=" fa fa-envelope"></i> beelal.business@gmail.com
                    </div>
                    <div class="phone-service">
                        <i class=" fa fa-phone"></i> +62852-1982-6788
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="inner-header">
                <div class="row">
                    <div class="col-lg-2 col-md-2">
                        <div class="logo">
                            <router-link to="/">
                                <img src="img/sipu-baby.png" alt="" />
                            </router-link>
                        </div>
                    </div>
                    <div class="col-lg-7 col-md-7"></div>
                    <div class="col-lg-3 text-right col-md-3">
                        <ul class="nav-right">
                            <li class="cart-icon">
                                Keranjang Belanja &nbsp;
                                <a href="#">
                                    <i class="icon_bag_alt"></i>
                                    <span>{{ keranjang.length }}</span>
                                </a>
                                <div class="cart-hover">
                                    <div class="select-items">
                                        <table>
                                            <tbody v-if="keranjang.length > 0"> 

                                                <tr v-for="keranjang in keranjang" :key="keranjang.id">
                                                    <td class="si-pic">
                                                        <img class="photo-item" :src="keranjang.image" alt="" />
                                                    </td>
                                                    <td class="si-text">
                                                        <div class="product-selected">
                                                            <p>Rp. {{ keranjang.price }}</p>
                                                            <h6>{{ keranjang.name }}</h6>
                                                        </div>
                                                    </td>
                                                    <td @click="removeItem(keranjang.id)" class="si-close">
                                                        <i class="ti-close"></i>
                                                    </td>
                                                </tr>

                                            </tbody>

                                            <tbody v-else>
                                                <tr>
                                                    <td>Keranjang Kosong</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <div class="select-total">
                                        <span>total:</span>
                                        <h5>Rp. {{ totalHarga }}</h5>
                                    </div>
                                    <div class="select-button">
                                        <router-link to="/shoping-cart" class="primary-btn view-card">VIEW CART</router-link>
                                        <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <!-- Header End -->
  </div>
</template>

<script>
export default {
    name: 'header',
    data() {
        return {
            keranjang: [],
        }
    },
    mounted() {
        if(localStorage.getItem('keranjang')){
            try {
                this.keranjang = JSON.parse(localStorage.getItem('keranjang'))
            } catch(e) {
                localStorage.removeItem('keranjang')
            }
        }   
    },
    computed: {
        totalHarga(){
            let total = 0
            for(let i = 0; i < this.keranjang.length; i++){
                total += (parseInt(this.keranjang[i].price))
            }
            return total
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
        }
    }

}
</script>

<style scoped>
    .photo-item {
        width: 80px;
        height: 80px;
    }
</style>