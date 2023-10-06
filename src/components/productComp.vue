<!-- componentProduct -->
<template>
  <div>
    <div class="box">
      <el-row class="textInTitle">
        <el-col :span="24">
          <div>
            {{ prod.title }}
          </div>
        </el-col>
        <el-col :span="24" class="brand">
          <span class="key">brand:</span>
          <span class="val">{{ prod.brand }}</span>
        </el-col>
        <el-col :span="24">
          <el-carousel indicator-position="outside">
            <el-carousel-item
              v-for="(img, index) in prod.images"
              :key="index"
              class="imgs"
            >
              <img :src="img" alt="" />
            </el-carousel-item>
          </el-carousel>
        </el-col>
        <el-col :span="24" v-if="$route.path === '/'">
          <div @mouseover="price = 'ADD+'" @mouseout="price = '₪' + prod.price">
            <el-button
              class="textPrice"
              id="price"
              @click="$emit('addcard', prod._id)"
            >
              {{ price }}</el-button
            >
          </div>
        </el-col>
        <el-col :span="24" class="rating">
          <el-rate
            class="rating"
            v-model="rating.rating"
            disabled
            show-score
            text-color="#ff9900"
            :score-template="`${rating.rating} points`"
          ></el-rate>
        </el-col>
        <el-col :span="24" v-if="$route.path !== '/'"
          >Amount: {{ count }}</el-col
        >
      </el-row>
    </div>
  </div>
</template>
<script>
export default {
  props: ["prod"],
  data() {
    return {
      rating: this.prod,
      count: this.prod.Some,
      price: "₪" + this.prod.price,
    };
  },
  watch: {
    value1(newVal) {
      // alert(newVal);
    },
  },
  mounted() {
    if (this.$route.path !== "/") {
    }
    // console.log("arr in comp prod");
  },

  methods: {
    ale() {
      console.log("Mouse over triggered");

      // alert();
    },
  },
};
</script>
<style scoped>
.box {
  display: flex;
  justify-content: center;
  height: 450px;
  padding: 30px;
  border-radius: 15px;
}
.textInTitle {
  height: 20px;
  text-align: center;
  border-bottom: 2px solid salmon;
  background: rgb(255, 199, 199);
  width: 100%;
}
.key {
  background: #909399;
  font-size: 20px;
  width: 100%;
}
.val {
  background: #606266;
  font-size: 20px;
  width: 100%;
}
.brand {
  position: absolute;
  top: 20px;
  display: flex;
}
.key,
.val {
  width: 50%;
  /* box-sizing: border-box; */
}
.imgs {
  position: absolute;
  top: 25px;
  height: 250px;
  width: auto;
}
#price {
  display: grid;
  font-size: 30px;
  background: rgb(95, 255, 130);
  width: 100%;
  border-radius: 20px;
  place-items: center;
}
/* .textPrice {
} */
</style>
<style>
body {
  margin-left: 0.3%;
  margin-right: 0.3%;
  margin-top: 11px;
  background: rgb(255, 248, 148);
}
</style>
