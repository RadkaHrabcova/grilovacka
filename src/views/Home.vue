<template>
  <div class="home">
    <appbar />
  </div>
</template>


<script>
import Appbar from "./../components/Appbar.vue";
export default {
  components: {
    Appbar
  },
  data(){
      return{
          selectedGrillId: '',
          selectedGrills:[],
          allFilters:[],
          allGrills:[ 
            {id: 0, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            {id: 1, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            {id: 2, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            {id: 3, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            {id: 4, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            {id: 5, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            {id: 6, name:'Veřejný gril Lužánky', lat: 49.2079947, lon:16.6066672, wc: true, parking: false, reservation: true, charge: true, sportsGround: true, playground: true}, 
            
            
            ],
      }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Amatic+SC&family=Roboto&display=swap");

body {
  font-family: "Roboto", sans-serif;
}
h1,
h2 {
  font-family: "Amatic SC", cursive;
}
</style>
