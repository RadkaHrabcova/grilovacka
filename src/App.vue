<template>
  <v-app id="keep">
    <v-app-bar app clipped-left color="#f3f5e1">
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-col md="3">
        <h1>Grilovačka</h1>
      </v-col>

      <v-col md="6">
        <v-card flat color="#f3f5e1">
          <v-row>
            <v-btn-toggle v-model="toggle_exclusive" multiple>
              <v-btn>
                <img v-bind:src="require('./assets/icons/grill.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/campfire.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/parking.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/reservation.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/toilet.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/wallet.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/playground.svg')" width="40" height="40" />
              </v-btn>
              <v-btn>
                <img v-bind:src="require('./assets/icons/football.svg')" width="40" height="40" />
              </v-btn>
            </v-btn-toggle>
          </v-row>
        </v-card>
      </v-col>

      <v-col md="4">
        <v-row>
          <v-btn class="ma-2" tile outlined>
            <h2>Zobrazit</h2>
          </v-btn>

          <v-btn class="ma-2" tile outlined>
            <h2>Resetovat vše</h2>
          </v-btn>
        </v-row>
      </v-col>
    </v-app-bar>

    <v-col md="8">
      <v-navigation-drawer v-model="drawer" app clipped color="grey lighten-4">
        <listOfPlaces
         
          v-bind:searchedGrills="searchedGrills"
          v-on:search-grills="showSearchedGrills($event)"
        />
      </v-navigation-drawer>
    </v-col>

    <v-footer>
      <v-card flat tile class="indigo lighten-1 black--text text-center">
        <v-card-text
          pt-0
        >Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit amet. Mauris cursus commodo interdum. Praesent ut risus eget metus luctus accumsan id ultrices nunc. Sed at orci sed massa consectetur dignissim a sit amet dui. Duis commodo vitae velit et faucibus. Morbi vehicula lacinia malesuada. Nulla placerat augue vel ipsum ultrices, cursus iaculis dui sollicitudin. Vestibulum eu ipsum vel diam elementum tempor vel ut orci. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</v-card-text>

        <v-divider></v-divider>

        <v-card-text>
          {{ new Date().getFullYear() }} —
          <strong>Radka a Markéta</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  </v-app>
</template>

<script>
import ListOfPlaces from "./components/ListOfPlaces.vue";

export default {
  components: {
    listOfPlaces: ListOfPlaces
  },
  props: {
    source: String
  },

   computed:{
      searchedGrills(){
        if (this.searchedText.length === 0){
        return this.allGrills
        }else{
          return this.allGrills.filter(grills => grills.name.toLowerCase().includes(this.searchedText.toLowerCase()))
        }
      }
     
    },

  methods: {
    showSearchedGrills(data) {
      this.searchedText = data
      
    },

   
  },
  data() {
    return {
      searchedText: "",
      

      drawer: null,
      headers: [
        {
          text: "11 výsledků",
          align: "start",
          sortable: false,
          value: "name"
        },
        { text: "Vzdálenost", value: "distance" },
        { text: "Hodnocení", value: "rating" },
        { text: "Navigace", value: "navigation" }
      ],
      allGrills: [
        {
          id: 0,
          name: "Veřejný gril Lužánky",
          lat: 49.2079947,
          lon: 16.6066672,
          wc: true,
          parking: false,
          reservation: true,
          charge: true,
          sportsGround: true,
          playground: true
        },
        {
          id: 1,
          name: "Veřejný gril Kraví hora",
          lat: 49.2035717,
          lon: 16.5842714,
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true
        },
        {
          id: 2,
          name: "Nový Lískovec",
          lat: 49.1754453,
          lon: 16.5487825,
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true
        },
        {
          id: 3,
          name: "Kozí horka",
          lat: 49.2386636,
          lon: 16.5052903,
          wc: true,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: false
        },
        {
          id: 4,
          name: "Veřejný gril Lužánky",
          lat: 49.2079947,
          lon: 16.6066672,
          wc: true,
          parking: false,
          reservation: true,
          charge: true,
          sportsGround: true,
          playground: true
        },
        {
          id: 5,
          name: "Veřejný gril Lužánky",
          lat: 49.2079947,
          lon: 16.6066672,
          wc: true,
          parking: false,
          reservation: true,
          charge: true,
          sportsGround: true,
          playground: true
        },
        {
          id: 6,
          name: "Veřejný gril Lužánky",
          lat: 49.2079947,
          lon: 16.6066672,
          wc: true,
          parking: false,
          reservation: true,
          charge: true,
          sportsGround: true,
          playground: true
        }
      ],
      
    };
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