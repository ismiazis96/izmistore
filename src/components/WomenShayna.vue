<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <!-- membuat kondisi jika product nya lebih dari 0/ kosong -->
        <div class="col-lg-12 mt-5" v-if="products.length > 0 ">
          <carousel
            class="product-slider"
            :item="3"
            :nav="false"
            :autoplay="true"
            :dots="false"
          >
            <!-- membuat looping menggunakn for, harus ada key atau kunci yang diawaliki dengan id -->
            <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
              <div class="pi-pic">
                <img v-bind:src="itemProduct.galleries[0].photo" alt="" />
                <ul>
                  <li class="w-icon active" @click="saveKeranjang(itemProduct.id,itemProduct.name,itemProduct.price,itemProduct.galleries[0].photo)">
                    <a href="#">
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link to=/product>
                <a href="#">
                  <h5>{{ itemProduct.name }}</h5>
                </a>
                </router-link>
                <div class="product-price">
                  ${{ itemProduct.price }}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
          <!-- jika tidak ada, maka kondisi akan dijalankan dibawahnya -->
        </div>
        
        <div class="col-lg-12" v-else>
         <p> Product terbaru belum tersedia.</p>
        </div>
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->

  
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "WomenShayna",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      keranjangUser:[]
    };
  },
  mounted() {
    axios
    .get("http://shayna-backend.belajarkoding.com/api/products")
    .then(res => (this.products = res.data.data.data))
    // eslint.disable-next-line no-console
    .catch(err => console.log(err));

    if (localStorage.getItem("keranjangUser")) {
      try{
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjanguser");
      }
    }
  },
    methods:  {
     saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      var ProductStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct,
      };

      this.keranjangUser.push(ProductStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);

      window.location.reload();
    },
  }
};
</script>

<style scoped>
.product-item {
  margin-right: 25px;
}
</style>
