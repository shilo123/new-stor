<!-- homeView -->
<template v-if="shogen">
  <div
    style="width: 100%; height: 100%; position: absolute"
    v-loading="loading"
    element-loading-text="Loading..."
    element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.8)"
  >
    <Menu
      :productso="products"
      :productsG="productsG"
      :option="option"
      :ArrIdso="ArrIds"
      @reload="re"
      @updateP="up"
    ></Menu>
    <el-row :gutter="16">
      <el-col :span="8" v-for="prod in products" :key="prod._id">
        <product :prod="prod" class="compo" @addcard="addcarD"></product>
      </el-col>
    </el-row>
    <!--        
 -->
  </div>
</template>
<script>
import product from "@/components/productComp.vue";
import Menu from "@/components/MenuAndLogo.vue";
import { URLSERVERA } from "@/URL/url";
import { URL } from "@/URL/url";
export default {
  components: { product, Menu },
  data() {
    return {
      loading: false,
      products: "",
      productsG: "",
      option: [],
      shogen: false,
      ArrIds: [],
    };
  },
  mounted() {
    this.loading = true;
    this.$ax.get(URLSERVERA).then((res) => {
      this.$ax.post(URL + "reqData", res.data.products).then((res) => {
        if (res.data.lenght === 0) location.reload();
        this.products = res.data;
        this.productsG = this.products;
        this.shogen = true;
        for (const key in this.products[0]) {
          if (key === "category" || key === "price" || key === "title") {
            this.option.push(key);
          }
        }
      });
      this.loading = false;
    });
  },

  methods: {
    up(dogy) {
      this.products = this.productsG;
      this.products = dogy;
      {
      }
    },
    addcarD(id) {
      let pro = this.products.find((e) => {
        return e._id === id;
      });
      this.$message.success(pro.title + "added to cart");
      this.ArrIds.push(pro._id);
    },
    re() {
      this.ArrIds = [];
    },
  },
};
</script>
<style scoped>
.compo {
  border: 2px solid darkblue;

  background: rgb(202, 202, 202);
  border-radius: 15px;
  margin-bottom: 13px;
}
</style>
