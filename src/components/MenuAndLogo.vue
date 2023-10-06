<template>
  <div>
    <!-- id -->
    <div class="otef">
      <div class="titleW">
        <strong class="textTitle"> Welcome to Stor</strong>
        <el-menu
          class="el-menu"
          default-active="activeIndex"
          mode="horizontal"
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#ffd04b"
        >
          <el-menu-item
            v-if="$route.path === '/'"
            index="1"
            style="width: 20%; font-size: 40px"
            @click="pushrout"
          >
            <el-badge
              :value="ArrIds.length"
              type="primary"
              class="el-icon-shopping-cart-2"
            >
              cart
            </el-badge>
          </el-menu-item>

          <el-menu-item
            index="2"
            style="float: right"
            v-if="$route.path === '/'"
          >
            <div>
              <el-select
                v-model="selectC"
                placeholder="Search by..."
                @input="selecto()"
              >
                <el-option
                  v-for="op in option"
                  :key="op"
                  :value="op"
                ></el-option>
                <el-option value="all"></el-option>
              </el-select>
            </div>
          </el-menu-item>
          <el-menu-item index="3" style="float: right" v-if="showInputText"
            ><el-input
              v-model="input"
              :placeholder="selectC"
              @input="filterText"
            ></el-input
          ></el-menu-item>
          <el-menu-item index="4" v-if="showInputTextC" style="float: right"
            ><el-input
              v-model="input"
              :placeholder="selectC"
              @input="filterText"
            ></el-input
          ></el-menu-item>
          <el-menu-item index="5" style="width: 30%" v-if="showInputPrice">
            <el-input
              v-model="inputNF"
              placeholder="price"
              @input="filterPrice"
            >
              <template slot="prepend">from</template>
            </el-input>
            <el-input
              v-model="inputNU"
              placeholder="price"
              @input="filterPrice"
            >
              <template slot="prepend">Until</template>
            </el-input>
          </el-menu-item>
          <el-menu-item
            index="6"
            class="el-icon-s-home"
            @click="pushHome"
            style="width: 20%; font-size: 40px"
            v-if="!showInputText && !showInputTextC && !showInputPrice"
            >Home</el-menu-item
          >
          <el-menu-item
            index="7"
            v-if="!showInputText && !showInputTextC && !showInputPrice"
            @click="refresh"
            style="width: 20%; font-size: 40px"
            class="el-icon-refresh"
            >refresh</el-menu-item
          >
          <!-- !showInputText || !showInputTextC || !showInputPrice -->
        </el-menu>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  //:products="products" :productsG="productsG" :option="option"
  props: ["productso", "productsG", "option", "ArrIdso"],
  data() {
    return {
      products: this.productso,
      selectC: "",
      showInputText: false,
      showInputTextC: false,
      showInputPrice: false,
      inputNF: 0,
      inputNU: "",
      input: "",
      ArrIds: this.ArrIdso,
    };
  },
  watch: {
    showInputText(newValue) {
      if (newValue === true) {
        this.showInputTextC = false;
        this.showInputPrice = false;
      }
    },
    showInputTextC(newValue) {
      if (newValue === true) {
        this.showInputText = false;
        this.showInputPrice = false;
      }
    },
    showInputPrice(newValue) {
      if (newValue === true) {
        this.showInputText = false;
        this.showInputTextC = false;
      }
    },
  },

  mounted() {},

  methods: {
    refresh() {
      this.$emit("reload");
      window.location.reload();
    },
    filterText() {
      this.products = this.productsG;
      if (this.selectC === "title") {
        if (this.input === "") {
          this.products = this.productsG;
        }
        if (this.input !== "") {
          this.products = this.products.filter((e) => {
            return e.title.includes(this.input);
          });
        }
      }
      if (this.selectC === "category") {
        if (this.input === "") {
          this.products = this.productsG;
        }
        if (this.input !== "") {
          this.products = this.products.filter((e) => {
            return e.category.includes(this.input);
          });
        }
      }
      this.$emit("updateP", this.products);
    },
    filterPrice() {
      this.products = this.productsG;
      this.products = this.products.filter((e) => {
        let unt = this.inputNU;
        if (this.inputNU === "") {
          unt = 99999;
        }
        return e.price > this.inputNF && e.price < unt;
      });
      // console.log("this.products", this.products);
      if (this.products.length === 0) {
        setTimeout(() => {
          this.$message.warning(
            "We did not find the price you were looking for"
          );
        }, 2000);
      }
      this.$emit("updateP", this.products);
    },
    selecto() {
      this.products = this.productsG;
      if (this.selectC === "title") {
        this.showInputText = true;
        this.filterText();
      }
      if (this.selectC === "price") {
        this.showInputPrice = true;
        this.filterPrice();
      }
      if (this.selectC === "category") {
        this.showInputTextC = true;
        this.filterText();
      }
      if (this.selectC === "all") {
        this.showInputText = false;
        this.showInputPrice = false;
        this.showInputTextC = false;
        this.selectC = "";
        this.products = this.productsG;
      }
    },
    pushrout() {
      this.ArrIds = this.ArrIds.join(",");
      console.log(this.ArrIds);
      if (this.ArrIds.length > 0) {
        this.$router.push({ path: "/Cart/" + this.ArrIds });
      } else {
        this.$message.error("מה אתה משוגע לא בחרת כלום!");
      }
    },
    pushHome() {
      if (this.$route.path !== "/") {
        this.$router.push({ path: "/" });
      }
    },
  },
};
</script>
<style scoped>
.otef {
  display: grid;
  place-items: center;
  text-align: center;
}
.titleW {
  width: 100%;
  font-size: 50px;
  position: relative;
  bottom: 10px;
  inline-size: 100%;
  background-image: url("https://www.korenvs.co.il/wp-content/uploads/2022/01/23-thumb.jpeg");
  height: 157px;
  z-index: 2;
}
.textTitle {
  background: rgb(192, 178, 52);
  border-radius: 20px;
}
.el-menu {
  position: relative;
  top: 31%;
  z-index: 1;
}
</style>
