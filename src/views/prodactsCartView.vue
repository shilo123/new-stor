<template>
  <div>
    <Menu :productso="products" :productsG="productsG" :ArrIdso="ArrIds"></Menu>
    <el-row :gutter="13">
      <el-col :span="8" v-for="prod in products" :key="prod._id">
        <product :prod="prod" class="compo" v-if="shogen"></product>
      </el-col>
    </el-row>
    <h1 class="h">Details of the purchase</h1>
    <el-table
      :data="products"
      :summary-method="getSummaries"
      show-summary
      v-if="shogen"
    >
      <el-table-column label="price" prop="price"></el-table-column>
      <el-table-column label="Price per unit" prop="priceP"></el-table-column>

      <el-table-column label="name" prop="title"></el-table-column>
      <el-table-column label="info" prop="description"></el-table-column>
      <el-table-column label="category" prop="category"></el-table-column>
      <el-table-column label="brand" prop="brand"></el-table-column>
      <el-table-column label="rating" prop="rating"></el-table-column>
      <el-table-column label="some" prop="Some"></el-table-column>
    </el-table>
    <el-form
      v-if="showF"
      :model="form"
      ref="form"
      label-width="120px"
      :rules="rules"
      class="from"
    >
      <el-form-item label="name" class="labelo" prop="name">
        <el-input v-model="form.name" placeholder="name"></el-input
      ></el-form-item>
      <el-form-item label="tz" class="labelo" prop="tz">
        <el-input v-model="form.tz" placeholder="tz"></el-input
      ></el-form-item>
      <el-form-item label="CardValidity" class="labelo" prop="Card">
        <el-select
          v-model="form.CardValidity.year"
          placeholder="CardValidity/year"
        >
          <el-option
            v-for="(v, i) in optionYear"
            :key="i"
            :label="v"
            :value="v"
          >
          </el-option>
        </el-select>
        <el-select
          v-model="form.CardValidity.month"
          placeholder="CardValidit/month"
        >
          <el-option
            v-for="(v, i) in optionMonth"
            :key="i"
            :label="v"
            :value="v"
          >
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="cvv" class="labelo" prop="cvv">
        <el-input v-model="form.cvv" placeholder="cvv"
          >cvv</el-input
        ></el-form-item
      >
      <el-form-item label="sum"
        ><el-input v-model="sum" placeholder="" disabled></el-input
      ></el-form-item>
      <el-button type="primary" style="width: 100%" @click="sub('form')"
        >sub</el-button
      >
    </el-form>
    <div v-if="!showF" style="text-align: center; font-size: 50px">
      The details have been sent
    </div>
    <router-link v-if="!showF" to="/" style="position: absolute; left: 44%"
      ><el-button type="primary">Go to home</el-button></router-link
    >
    <!--  -->
  </div>
</template>
<script>
import Menu from "@/components/MenuAndLogo.vue";
import product from "@/components/productComp.vue";
import { URL } from "@/URL/url";

export default {
  components: { Menu, product },

  data() {
    return {
      showF: true,
      products: [],
      productsG: [],
      ArrIds: [],
      shogen: true,
      sum: "",
      form: {
        name: "",
        tz: "",
        CardValidity: {
          year: "",
          month: "",
        },
        cvv: "",
      },
      optionYear: ["20", "21", "22", "23", "25", "26", "27", "28", "29", "30"],
      optionMonth: [
        "01",
        "02",
        "03",
        "04",
        "05",
        "06",
        "07",
        "08",
        "09",
        "10",
        "11",
        "12",
      ],
      rules: {
        name: [
          {
            required: true,
            message: "Please input Activity name",
            trigger: "blur",
          },
          {
            min: 3,
            max: 10,
            message: "Length should be 3 to 10",
            trigger: "blur",
          },
        ],
        tz: [
          {
            required: true,
            message: "please input Activ",
            trigger: "blur",
          },
          {
            min: 8,
            max: 10,
            message: "Length should be 8 to 10",
            trigger: "blur",
          },
        ],
        cvv: [
          {
            required: true,
            message: "please input Activ",
            trigger: "blur",
          },
          {
            min: 3,
            max: 3,
            message: "Length should be 3",
            trigger: "blur",
          },
        ],
        // Card: [
        //   {
        //     required: true,
        //     message: "Please select Activity zone",
        //     trigger: "blur",
        //   },
        // ],
      },
    };
  },
  mounted() {
    this.products = this.productsG;
    this.ArrIds = [];
    let id = this.$route.params.id;
    id = id.split(",");
    console.log("id", id);
    this.ArrIds = id;
    this.$ax.get(URL + "shoko").then((res) => {
      this.products = res.data;
      this.productsG = res.data;

      this.products = this.products.filter((e, ind) => {
        return this.ArrIds.includes(e._id.toString());
      });
      let countObj = {};
      this.ArrIds.forEach((item) => {
        countObj[item] = (countObj[item] || 0) + 1;
      });
      this.products.forEach((element, i) => {
        if (element._id in countObj) {
          Object.defineProperty(element, "Some", {
            value: countObj[element._id],
            writable: true,
            enumerable: true,
            configurable: true,
          });
        }
      });
      this.products.forEach((element, i) => {
        Object.defineProperty(element, "priceP", {
          value: element.price,
          writable: true,
          enumerable: true,
          configurable: true,
        });
      });

      //priceP
      this.products.map((e) => {
        return (e.price = e.price * e.Some);
      });
      let sum = 0;
      //   alert(sum);
      this.products.forEach((element) => {
        sum += element.price;
      });
      this.sum = sum;
      console.log("this.products", this.products);
    });
  },

  methods: {
    getSummaries() {
      return [
        "sum:" + " " + this.sum,
        "quantity of products:" + " " + this.ArrIds.length,
      ];
    },
    sub(from) {
      this.theValide(from);
    },
    theValide(from) {
      this.$refs[from].validate((valid) => {
        if (valid) {
          this.showF = false;
          this.$ax.post(URL + "ashrai", this.form).then((res) => {});

          this.$notify({
            title: "hoy",
            message: "The details have been sent",
            type: "success",
          });
        } else {
          this.$message.error("אוי לנו");
          setTimeout(() => {
            this.$refs[from].resetFields();
          }, 4000);
          return false;
        }
      });
    },
  },
};
</script>
<style scoped>
.compo {
  background: rgb(68, 225, 170);
  border-radius: 16px;
  height: 450px;
  margin-bottom: 5px;
}
.from {
  position: absolute;
  left: 24%;
  width: 50%;
  border: 3px solid rgb(186, 179, 105);
  border-radius: 15px;
  background: rgb(186, 179, 105);
  padding: 25px;
}
.labelo {
  color: black;
}
.h {
  text-align: center;
  background: rgb(108, 179, 162);
  height: 110px;
  margin: 0%;
}
</style>
<style>
body {
  margin-bottom: 10px;
}
</style>
