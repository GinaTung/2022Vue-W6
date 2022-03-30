<template>
  <h2>單一產品</h2>
  <div class="container">
    <!-- {{ product }} -->
    <template v-if="product.id">
      <div class="card mb-3">
        <img
          :src="product.imageUrl"
          class="card-img-top primary-image"
          alt="主圖"
        />
        <div class="card-body">
          <h5 class="card-title">
            {{ product.title }}
            <span class="badge bg-primary ms-2">{{
              product.category
            }}</span>
          </h5>
          <p class="card-text">商品描述：{{ product.description }}</p>
          <p class="card-text">商品內容：{{ product.content }}</p>
          <div class="d-flex">
            <p class="card-text me-2">{{ product.price }}</p>
            <p class="card-text text-secondary">
              <del>{{ product.origin_price }}</del>
            </p>
            {{ product.unit }} / 元
          </div>
        </div>
      </div>
      <!-- <template v-for="item in product.imagesUrl">
        <img v-bind:src="item" alt="" class="images m-2" />
      </template> -->
    </template>
    <p class="text-secondary" v-else>請選擇一個商品查看</p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      product: []
    }
  },
  methods: {
    getProduct () {
      // $router 方法
      // $route 物件->取值
      console.log(this.$route)
      const { id } = this.$route.params
      this.$http(
        `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/product/${id}`
      ).then((res) => {
        this.product = res.data.product
      })
    }
  },
  mounted () {
    this.getProduct()
  }
}
</script>
