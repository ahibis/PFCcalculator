<template>
  <v-form ref="form">
    <v-text-field
      label="продукт"
      v-model="newProduct.name"
      :rules="nameRules"
      hide-details="auto"
      counter="40"
      hint="название продукта"
      required
    ></v-text-field>
    <v-text-field
      label="Калории (Ккал)"
      type="number"
      hide-details
      v-model="newProduct.calories"
      required
    ></v-text-field>
    <v-text-field
      label="белки (г)"
      type="number"
      hide-details
      v-model="newProduct.proteins"
      required
    ></v-text-field>
    <v-text-field
      type="number"
      hide-details
      label="жиры"
      v-model="newProduct.fats"
      required
    ></v-text-field>
    <v-text-field
      label="углеводы (г)"
      type="number"
      hide-details
      v-model="newProduct.carbs"
      required
    ></v-text-field>
    <v-btn color="cyan lighten-4" type="submit" @click="addProduct"
      >Добавить</v-btn
    >
  </v-form>
</template>

<script>
export default {
  data(){
    return {
      nameRules: [
        (v) => (v && v.length >= 2) || "имя должно быть больше 2 символов",
        (v) => (v && v.length <= 40) || "имя должно быть меньше 40 символов",
        (v) => !vm.products.find(e=>e.name==v) || "данный продукт уже существует",
        v=>/^[А-Яа-яa-zA-Z\s]*$/u.test(v) || "имя может содержать только русские, английские буквы и пробел"
      ],
      newProduct: {
        name: "",
        calories: 50,
        proteins: 50,
        fats: 50,
        carbs: 50,
      },
    }
  },
  methods:{
    async addProduct() {
      const form = this.$refs.form;
      let validate = await form.validate();
      if (!validate.valid) return;
      const { newProduct } = this;
      this.$emit("addProduct",newProduct)
      newProduct.name = ""
    },
  }
};
</script>