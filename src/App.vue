<template>
  <v-app id="keep">
    <div class="my-wrapper">
      <v-row class="flex-nowrap" no-gutters>
        <div style="height: 100px; width:100%" floating class="menu-wrapper" height="30px">
          <v-row class="justify-space-between align-center fill-height px-4" no-gutters>
            <div>
              <v-row no-gutters class="align-center">
                <v-app-bar-nav-icon s @click="drawer = !drawer"></v-app-bar-nav-icon>
                <h1>Grilovačka</h1>
              </v-row>
            </div>

            <div>
              <v-btn-toggle v-model="toggle" multiple color="#f3f5e1">
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/grill.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/campfire.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/parking.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/reservation.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/toilet.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/wallet.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/playground.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
                <v-btn>
                  <img
                    class="filterIcon"
                    v-bind:src="require('./assets/icons/football.svg')"
                    width="40"
                    height="40"
                  />
                </v-btn>
              </v-btn-toggle>
            </div>

            <div>
              <!-- <v-col>
              <v-btn tile outlined>
                <h2>Zobrazit</h2>
              </v-btn>
              </v-col>-->

              <v-btn tile outlined @click="resetovat">
                <h2>Resetovat vše</h2>
              </v-btn>
            </div>
          </v-row>
        </div>
      </v-row>

      <div>
        <v-row class="flex-nowrap">
          <div style="overflow: scroll; height: 80vh; width: 40vw">
            <ListOfPlaces
              @id="((idGrill) => selectedGrill=idGrill)"
              v-if="!selectedGrillObject"
              v-bind:searchedGrills="searchedGrills"
              v-on:search-grills="showSearchedGrills($event)"
            />

            <detail
              v-else
              v-bind:oneGrill="selectedGrillObject"
              @closeDetail="(() => selectedGrill=null)"
            />
          </div>
          <div style="height: 80vh;
    width: 100%;">
            <MyMap
              v-bind:oneGrill="selectedGrillObject"
              :grills="searchedGrills"
              @indexDetail="((index)=> selectedGrill=index)"
              ref="myMap"
            />
          </div>
        </v-row>
      </div>
      <div>
        <v-footer>
          <v-card flat tile class="lighten-1 black--text text-center">
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
      </div>
    </div>
  </v-app>
</template>

<script>
import MyMap from "./components/Map";
import { latLng } from "leaflet";
import ListOfPlaces from "./components/ListOfPlaces.vue";
import Detail from "./components/Detail.vue";

export default {
  components: {
    ListOfPlaces,
    MyMap,
    Detail
  },
  props: {
    source: String
  },
  watch: {
    selectedGrill: function(val) {
      let position = null;
      for (let grill of this.allGrills) {
        if (val === grill.id) {
          position = grill.position;
        }
      }
      this.$refs.myMap.setcenter(position);
    }
  },

  computed: {
    searchedGrills() {
      let filteredGrills = this.allGrills;

      if (this.searchedText.length > 0) {
        filteredGrills = filteredGrills.filter(grills =>
          grills.name.toLowerCase().includes(this.searchedText.toLowerCase())
        );
      }

      if (this.toggle.some(item => item === 4)) {
        // WC
        filteredGrills = filteredGrills.filter(grill => grill.wc);
      }
      if (this.toggle.some(item => item === 2)) {
        //parking
        filteredGrills = filteredGrills.filter(grill => grill.parking);
      }
      if (this.toggle.some(item => item === 3)) {
        //reservation
        filteredGrills = filteredGrills.filter(grill => grill.reservation);
      }
      if (this.toggle.some(item => item === 5)) {
        //charge
        filteredGrills = filteredGrills.filter(grill => grill.charge);
      }
      if (this.toggle.some(item => item === 6)) {
        //playground
        filteredGrills = filteredGrills.filter(grill => grill.playground);
      }
      if (this.toggle.some(item => item === 7)) {
        //sportsGround
        filteredGrills = filteredGrills.filter(grill => grill.sportsGround);
      }
      if (this.toggle.some(item => item === 0)) {
        //grill
        filteredGrills = filteredGrills.filter(grill => grill.type === "grill");
      }
      if (this.toggle.some(item => item === 1)) {
        //campfire
        filteredGrills = filteredGrills.filter(
          grill => grill.type === "campfire"
        );
      }

      return filteredGrills;
    },

    selectedGrillObject() {
      return this.allGrills.find(grill => grill.id === this.selectedGrill);
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
      currentLatLng:latLng(49.2079947, 16.6066672),
      toggle: [],
      searchedText: "",
      selectedGrill: null,
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
          type: "grill",
          rating: [3, 5, 3],
          wc: true,
          parking: false,
          reservation: true,
          charge: true,
          sportsGround: true,
          playground: true,
          grillImage: require("./assets/photos/01_Luzanky1.jpeg"),
          parkingInfo: "parkoviště 100 m",
          palivoInfo: " gril je elektrický",
          sportsGroundInfo: " venkovní posilovna",
          playgroundInfo: " pískoviště, houpačky",
          comments: [
            { date: "20.6.", text: "bylo to hezke misto" },
            { date: "20.5.", text: "hrozne" }
          ],
          distance: latLng(49.2079947, 16.6066672).distanceTo(latLng(49.2079947, 16.6066672))
        },

        {
          id: 2,
          name: "Veřejný gril Kraví hora",
          position: latLng(49.2035717, 16.5842714),
          type: "grill",
          rating: [5, 5, 2],
          wc: false,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: true,
          comments: [],
          distance: latLng(49.2035717, 16.5842714).distanceTo(latLng(49.2079947, 16.6066672))
        },
        {
          id: 3,
          name: "Nový Lískovec",
          position: latLng(49.1754453, 16.5487825),
          type: "grill",
          rating: [1, 3, 3],
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: false,
          comments: []
        },
        {
          id: 4,
          name: "Kozí horka",
          position: latLng(49.2386636, 16.5052903),
          type: "grill",
          rating: [5, 5, 1],
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: true,
          comments: []
        },
        {
          id: 5,
          name: "pod Dymou",
          position: latLng(49.2430192, 16.4991761),
          type: "grill",
          rating: [5, 5, 5],
          wc: false,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: false,
          comments: []
        },
        {
          id: 6,
          name: "Rokle",
          position: latLng(49.2480253, 16.4937339),
          type: "grill",
          rating: [1, 4, 3],
          wc: false,
          parking: true,
          reservation: true,
          charge: false,
          sportsGround: false,
          playground: true,
          comments: []
        },
        {
          id: 7,
          name: "Maloměřice",
          position: latLng(49.2270731, 16.6435522),
          type: "campfire",
          rating: [1, 1, 3],
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: false,
          comments: []
        },
        {
          id: 8,
          name: "Rakovec",
          position: latLng(49.2275075, 16.5104231),
          type: "campfire",
          rating: [1, 2, 3],
          wc: true,
          parking: true,
          reservation: true,
          charge: false,
          sportsGround: true,
          playground: false,
          comments: []
        }
      ]
    };
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Amatic+SC&family=Roboto&display=swap");

.v-btn-toggle > .v-btn.v-btn--active {
  color: black !important;
}

.filterIcon {
  padding: 5px;
}
.v-app-bar--fixed {
  z-index: 10000000 !important;
}

.menu-wrapper {
  background: #f3f5e1;
}

.my-wrapper {
  height: 100vh;
  overflow: hidden;
  flex-direction: column !important;
  flex-wrap: nowrap;
}

body {
  font-family: "Roboto", sans-serif;
}
h1,
h2 {
  font-family: "Amatic SC", cursive;
}
</style>