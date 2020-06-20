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
            <v-btn-toggle v-model="toggle" multiple>
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
          <!-- <v-btn class="ma-2" tile outlined>
            <h2>Zobrazit</h2>
          </v-btn>-->

          <v-btn class="ma-2" tile outlined v-on:click="resetovat">
            <h2>Resetovat vše</h2>
          </v-btn>
        </v-row>
      </v-col>
    </v-app-bar>

    <v-col md="8">
      <v-navigation-drawer v-model="drawer" app clipped color="grey lighten-4" width= '500'>
        <listOfPlaces
          v-bind:searchedGrills="searchedGrills"
          v-on:search-grills="showSearchedGrills($event)"
        />
      </v-navigation-drawer>
    </v-col>

    <MyMap :grills="searchedGrills" />

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
import MyMap from "./components/Map";
import { latLng } from "leaflet";
import ListOfPlaces from "./components/ListOfPlaces.vue";

export default {
  components: {
    listOfPlaces: ListOfPlaces,
    MyMap
  },
  props: {
    source: String
  },

  computed: {
    searchedGrills() {
      let filteredGrills = this.allGrills;

      if (this.searchedText.length > 0) {
        filteredGrills = filteredGrills.filter(grills =>
          grills.name.toLowerCase().includes(this.searchedText.toLowerCase())
        );
      }

      console.log("this.toggle", this.toggle);

      if (this.toggle.some(item => item === 4)) {
        // WC
        console.log("filtruji");
        filteredGrills = filteredGrills.filter(grill => grill.wc);
      } if(this.toggle.some(item => item === 2)) {
        //parking
        filteredGrills = filteredGrills.filter(grill => grill.parking);
      
      } if(this.toggle.some(item => item === 3)) {
        //reservation
        filteredGrills = filteredGrills.filter(grill => grill.reservation);
      
      } if(this.toggle.some(item => item === 5)) {
        //charge
        filteredGrills = filteredGrills.filter(grill => grill.charge);
      
      } if(this.toggle.some(item => item === 6)) {
        //playground
        filteredGrills = filteredGrills.filter(grill => grill.playground);
      
      } if(this.toggle.some(item => item === 7)) {
        //sportsGround
        filteredGrills = filteredGrills.filter(grill => grill.sportsGround);
      } if(this.toggle.some(item => item === 0)) {
        //grill
        filteredGrills = filteredGrills.filter(grill => grill.type === 'grill');
      } if(this.toggle.some(item => item === 1)) {
        //campfire
        filteredGrills = filteredGrills.filter(grill => grill.type === 'campfire');
      }

      return filteredGrills;
    }
  },

  methods: {
    resetovat() {
      this.toggle = [];
    },
    showSearchedGrills(data) {
      this.searchedText = data;
    }
  },
  data() {
    return {
      toggle: [],
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
          position: latLng(49.2079947, 16.6066672),
          type: 'grill', 
          rating: [3, 5 , 3], 
          wc: true,
          parking: false,
          reservation: true,
          charge: true,
          sportsGround: true,
          playground: true
        },
        {
          id: 1,
          name: "Veřejný gril Lužánky",
          position: latLng(49.2079947, 16.6066672),
          type: 'grill',
          rating: [3, 3 , 3],  
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true
        },
        {
          id: 2,
          name: "Veřejný gril Kraví hora",
          position: latLng(49.2035717, 16.5842714),
          type: 'grill',
          rating: [5, 5 , 2],  
          wc: false,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: true
        },
        {
          id: 3,
          name: "Nový Lískovec",
          position: latLng(49.1754453, 16.5487825),
          type: 'grill', 
          rating: [1, 3 , 3], 
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: false
        },
        {
          id: 4,
          name: "Kozí horka",
          position: latLng(49.2386636, 16.5052903),
          type: 'grill',
          rating: [5, 5 , 1],  
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: true
        },
        {
          id: 5,
          name: "pod Dymou",
          position: latLng(49.2430192, 16.4991761),
          type: 'grill', 
          rating: [5, 5 , 5], 
          wc: false,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: false
        },
        {
          id: 6,
          name: "Rokle",
          position: latLng(49.2480253, 16.4937339),
          type: 'grill', 
          rating: [1, 4 , 3], 
          wc: false,
          parking: true,
          reservation: true,
          charge: false,
          sportsGround: false,
          playground: true
        },
        {
          id: 7,
          name: "Maloměřice",
          position: latLng(49.2270731, 16.6435522),
          type: 'campfire', 
          rating: [1, 1 , 3], 
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: false
        },
        {
          id: 8,
          name: "Rakovec",
          position: latLng(49.2275075, 16.5104231),
          type: 'campfire',
          rating: [1, 2 , 3],  
          wc: true,
          parking: true,
          reservation: true,
          charge: false,
          sportsGround: true,
          playground: false
        }
      ]
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