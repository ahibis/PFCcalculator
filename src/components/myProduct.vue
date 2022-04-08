<template>
  <h2>Продукты на {{ dateName }}</h2>
  <v-table class="bd">
    <thead>
      <tr>
        <th class="text-left">Name</th>
        <th class="text-center">mass (g)</th>
        <th class="text-center">Calories</th>
        <th class="text-center">Protein (g)</th>
        <th class="text-center">Fat (g)</th>
        <th class="text-center">Carbs (g)</th>
        <th class="text-center"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, i) in dayProducts" :key="item.name">
        <td>{{ item.name }}</td>
        <td >

              <v-text-field
              @change="saveDay()"
              label="mass"
              filled 
              single-line
              type="number"
              v-model="item.mass"
              step="50"
              hide-details="auto"
            ></v-text-field>
            
        </td>
        <td>{{ ((item.calories * item.mass) / 100).toPrecision(4) }}</td>
        <td>{{ ((item.proteins * item.mass) / 100).toPrecision(4) }}</td>
        <td>{{ ((item.fats * item.mass) / 100).toPrecision(4) }}</td>
        <td>{{ ((item.carbs * item.mass) / 100).toPrecision(4) }}</td>
        <td>
          <v-btn
            class="mx-2"
            icon
            small
            color="primary"
            @click="deleteProduct(i)"
          >
            <v-icon> mdi-delete </v-icon>
          </v-btn>
        </td>
      </tr>
      <tr v-if="allProducts.calories">
        <td>в итоге</td>
        <td>{{ allProducts.mass.toPrecision(4) }}</td>
        <td>{{ allProducts.calories.toPrecision(4) }}</td>
        <td>{{ allProducts.proteins.toPrecision(4) }}</td>
        <td>{{ allProducts.fats.toPrecision(4) }}</td>
        <td>{{ allProducts.carbs.toPrecision(4) }}</td>
        <td></td>
      </tr>
    </tbody>
  </v-table>
  <v-row>
    <v-col xs="9">
      <v-autocomplete
        v-model="newValue"
        :items="productsName"
        dense
        filled
        hide-details="auto"
        label="добавить продукт"
      ></v-autocomplete>
    </v-col>
    <v-col xs="3">
      <v-btn icon color="blue" @click="addProduct" dense hide-details="auto">
        <v-icon> mdi-plus</v-icon>
      </v-btn>
    </v-col>
  </v-row>
  <v-row>
    <v-col xs="9">
      <v-text-field
        type="date"
        hide-details="auto"
        v-model="date"
        @change="loadDay()"
        dense
      ></v-text-field>
    </v-col>
    <v-col>
      <v-btn icon color="blue" @click="lastDay()"
        ><v-icon>mdi-minus</v-icon></v-btn
      >
      <v-btn icon color="blue" @click="nextDay()"
        ><v-icon>mdi-plus</v-icon></v-btn
      >
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: ["products"],
  data: () => ({
    dayProducts: [],
    newValue: "",
    date: "",
  }),
  methods: {
    addProduct() {
      console.log(1);
      const name = this.newValue;
      if (this.dayProducts.find((e) => e.name == name)) return;
      const product = this.products.find((e) => e.name == name);
      if (!product) return;
      this.dayProducts.push({ ...product, mass: 100 });
      this.saveDay();
    },
    loadDay() {
      const day = this.date;
      const data = localStorage[`product${day}`];
      this.dayProducts = data ? JSON.parse(data) : [];
    },
    saveDay() {
      const day = this.date;
      localStorage[`product${day}`] = JSON.stringify(this.dayProducts);
    },
    deleteProduct(i) {
      this.dayProducts.splice(i, 1);
      this.saveDay();
    },
    nextDay() {
      const { date } = this;
      const day = moment(date).add(1, "d");
      this.date = day.format("yyyy-MM-DD");
      this.loadDay();
    },
    lastDay() {
      const { date } = this;
      const day = moment(date).subtract(1, "d");
      this.date = day.format("yyyy-MM-DD");
      this.loadDay();
    },
  },
  mounted() {
    this.date = moment().format("yyyy-MM-DD");
    this.loadDay();
  },
  computed: {
    allProducts() {
      const res = {};
      this.dayProducts.forEach((product) => {
        let k = product.mass / 100;
        for (let [key, val] of Object.entries(product)) {
          if (key == "mass") {
            res[key] = res[key] ? (res[key] += val) : val;
            continue;
          }
          res[key] = res[key] ? (res[key] += val * k) : val * k;
        }
      });
      return res;
    },
    productsName() {
      return this.products.map((e) => e.name);
    },
    dateName() {
      return moment(this.date).format("DD.MM.YYYY");
    },
  },
};
</script>
<style>
</style>