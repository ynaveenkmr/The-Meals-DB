<template>
  <div class="dashboard">
    <b-container class="bv-example-row">
  <b-row >
    <b-col>
      <b-jumbotron>
      <h3>Menu</h3>
<b-list-group v-for="(menuList,idMeal) in meals" :key="idMeal">
  <b-list-group-item>{{menuList.strMeal}}</b-list-group-item>
</b-list-group>
</b-jumbotron>
    </b-col>
    <b-col>
      <b-jumbotron>
    <input type="text" v-model="searchByName" placeholder="Search Meal">
    <button @click="getData()">Search</button>
    <header class="table-info">
    </header>

    <table>
      <thead>
        <tr>
          <th v-for="(col,i) in mealCols" :key="i">{{ col }}</th>
        </tr>
      </thead>
      <tbody v-if="meals.length=1">
        <tr v-for="(meal,idMeal) in meals" :key="idMeal">
          <td>{{ meal.idMeal }}</td>
          <td>
            <img class="avatar" :src="meal.strMealThumb" alt>
          </td>
          <td>{{ meal.strMeal }}</td>
          <td>{{ meal.strArea }}</td>
          <td>
            <div>
              <b-button v-b-modal.modal-scrollable>Ingredients</b-button>

              <b-modal
                ok-only
                no-stacking
                id="modal-scrollable"
                scrollable
                title="Scrollable Content"
              >{{meal.strInstructions }}       
              </b-modal>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    </b-jumbotron>
    </b-col>
  </b-row>
</b-container>
   
  </div>
</template>
<script>
export default {
  name:"dashboard",
  data() {
    return {
      baseUrl: "https://www.themealdb.com/api/json/v1/1/search.php",
      meals: [],
      mealCols: ["Id", "Meal", "Meal Name", "Area"],
      menuList:[],
      searchByName: ""
    };
  },
  created(){
    this.getData();
  },
  methods: {
    async getData() {
      this.items = await fetch(`${this.baseUrl}?s=${this.searchByName}`)
        .then(response => response.json())
        .then(result => {
          this.meals = result.meals;
        });
    }
  }
}
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css?family=Nunito:400,600,700");
html {
  font-size: 16px;
  font-weight: 400;
  line-height: 1.5;
}

body,
button,
input,
select,
textarea {
  font-family: "Nunito", sans-serif;
}

#app {
  display: flex;
  justify-content: center;
}

.table-info {
  display: flex;
  justify-content: space-between;
}

.avatar {
  width: 64px;
}

table {
  border-collapse: collapse;
}

td {
  border-top: 1px solid #dee2e6;
}

th,
td {
  padding: 0.5rem;
}
</style>
