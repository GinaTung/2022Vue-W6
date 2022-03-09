<template>
  <div class="container">
    <table class="table align-middle">
      <thead>
        <tr>
          <th>圖片</th>
          <th>商品名稱</th>
          <th>價格</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td style="width: 200px">
            <div
              :style="{ backgroundImage: `url(${product.imageUrl})` }"
              style="
                height: 100px;
                background-size: cover;
                background-position: center;
              "
            ></div>
          </td>
          <td>
            {{ product.title }}
          </td>
          <td>
            <div class="h5" v-if="product.price === product.origin_price">
              {{ product.price }} 元
            </div>
            <div v-else>
              <del class="h6">原價 {{ product.origin_price }} 元</del>
              <div class="h5">現在只要 {{ product.price }} 元</div>
            </div>
          </td>
          <td>
            <div class="btn-group btn-group-sm">
              <button
                type="button"
                class="btn btn-outline-secondary"
                @click="openProductModal(product.id)"
                :disabled="isLoadingItem === product.id"
              >
                <!-- <i class="fas fa-spinner fa-pulse"></i> -->
                查看更多
              </button>
              <button
                type="button"
                class="btn btn-outline-danger"
                @click="addToCart(product.id)"
                :disabled="isLoadingItem === product.id"
              >
                <!-- <i class="fas fa-spinner fa-pulse"
                      ></i> -->
                <span
                  class="spinner-border spinner-border-sm"
                  v-show="isLoadingItem === product.id"
                ></span>
                加到購物車
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  datac () {
    return {
      cartData: {},
      products: [],
      isLoadingItem: ''
    }
  },
  methods: {
    getProducts () {
      this.$http(
        `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`
      )
        .then((res) => {
          this.products = res.data.products
          console.log(res)
        })
        .catch((err) => {
          alert(err.data.message)
        })
    },
    addToCart (id, qty = 1) {
      const data = {
        product_id: id,
        qty
      }
      this.isLoadingItem = id
      this.$http(
        `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`,
        { data }
      )
        .then((res) => {
          console.log(res)
          this.getCart()
          this.$refs.productModal.closeModal()
          this.isLoadingItem = ''
        })
        .catch((err) => {
          alert(err.data.message)
        })
    }
  },

  mounted () {
    this.getProducts()
  }
}
</script>
