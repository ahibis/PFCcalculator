<template>
  <h2>База данных</h2> 
  <v-table class="bd" v-if="bdShow">
    <thead>
      <tr>
        <th class="text-left">Name</th>
        <th class="text-center">Calories</th>
        <th class="text-center">Protein (g)</th>
        <th class="text-center">Fat (g)</th>
        <th class="text-center">Carbs (g)</th>
        <th class="text-center"></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, i) in products" :key="item.name">
        <td>{{ item.name }}</td>
        <td>{{ item.calories }}</td>
        <td>{{ item.proteins }}</td>
        <td>{{ item.fats }}</td>
        <td>{{ item.carbs }}</td>
        <td>
          <v-btn
            icon
            color="primary"
            @click="$emit('deleteProduct',i)"
          >
            <v-icon> mdi-delete </v-icon>
          </v-btn>
        </td>
      </tr>
    </tbody>
  </v-table>
  <v-divider></v-divider>
</template>
<script>
    export default {
        emits:["deleteProduct"],
        props:["products"],
        data:()=>({
          bdShow:true
        })
    }
</script>
<style>
  .bd{
    height: 35vh;
    overflow-y: auto;
  }
</style>