<template>
  <div class="container">
    <h2 class="mt-2">產品列表</h2>
    <div class="d-flex">
      <div class="product-content col-4">
        <div class="card" style="width: 18rem">
          <div class="card-header">全部 ({{ products.length }})</div>
          <ul class="list-group list-group-flush pr-1">
            <li class="list-group-item">戒指 ({{ products.length }})</li>
            <li class="list-group-item">寶石</li>
            <li class="list-group-item">項鍊</li>
            <li class="list-group-item">髮飾</li>
            <li class="list-group-item">耳環</li>
          </ul>
        </div>
      </div>
      <div class="card-item">
        <div class="row row-cols-1 row-cols-lg-4 g-2">
          <!--決定內層數量-->
          <!-- 內層不定義寬度 -->
          <div class="col-4" v-for="product in products" :key="product.id">
            <div class="card h-100">
              <img :src="product.imageUrl" class="card-img-top" alt="..." />
              <div class="card-body">
                <h5 class="card-title">{{ product.title }}</h5>
                <p class="card-text">{{ product.description }}</p>
                <router-link
                  :to="`/product/${product.id}`"
                  class="btn btn-primary"
                >
                  Go somewhere
                </router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    getProducts () {
      this.$http
        .get(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`
        )
        .then((res) => {
          this.products = res.data.products
        })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
