<template>
  <div class="home">
    <heading>
     Random Meals
    </heading>
   <div v-for="(randomImg,i) in meals" :key="i">
     <b-img
      style="height:400px;width:400px;background-size: cover;object-fit: contain;"
      :src="randomImg.strMealThumb"
    ></b-img>
    </div>
  </div>
</template>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
import axios from "axios";
export default {
  name: 'Home',
   props: {
    msg: String
  },
  data() {
    return {
      image: { url: "" },
      timer: "",
      meals:[]
    };
  },
  created() {
    this.getData();
   this.timer = setInterval(this.getData, 5000);
  },
  methods: {
   async getData() {
      this.items = await fetch("https://www.themealdb.com/api/json/v1/1/random.php")
        .then(response => response.json())
        .then(result => {
          this.meals = result.meals;
        });
    },
     cancelAutoUpdate() {
      clearInterval(this.timer);
    }
},
 beforeDestroy() {
    clearInterval(this.timer);
  }
}
</script>
