<template>
  <v-app id="keep">
    <div class="my-wrapper">
      <v-row class="flex-nowrap" no-gutters>
        <div style="height: 100px; width:100%" floating class="menu-wrapper" height="30px">
          <v-row class="justify-space-between align-center fill-height px-4" no-gutters>
            <div>
              <v-row no-gutters class="align-center">
                <v-app-bar-nav-icon s @click="drawer = !drawer">
                  <img :src="require('./assets/icons/greyPin.png')" height="40" />
                </v-app-bar-nav-icon>

                <v-row class="logo">
                  <img v-bind:src="require('./assets/redG.png')" height="80" />
                  <h1 class="logoText">rilovačka</h1>
                </v-row>
              </v-row>
            </div>
             
            <div  style="d-flex">
              
              <v-btn-toggle v-model="toggle" multiple>
                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                     <div >
              <!-- <h2>filtry</h2> -->
              
               <h2 >Vyber, co chceš mít na místě k dispozici:</h2> 
              
              </div >
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/grill.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>grill</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 40px 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/campfire.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>ohniště</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/parking.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>parkoviště</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/reservation.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>rezervace</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/wc-sign.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>wc</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/dollar-sign.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>s poplatkem</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/playground.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>dětské hřiště</span>
                </v-tooltip>

                <v-tooltip bottom z-index="1000">
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn style="margin: 0px 10px" v-bind="attrs" v-on="on">
                      <img
                        class="filterIcon"
                        v-bind:src="require('./assets/icons/field.svg')"
                        width="40"
                        height="40"
                      />
                    </v-btn>
                  </template>
                  <span>sportovní hřiště</span>
                </v-tooltip>
              </v-btn-toggle>
            </div>

            <div>
              <v-btn tile outlined @click="resetovat" color="#E0DCDC">
                <h2 class="reset">Resetovat vše</h2>
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
        <v-footer class="footer justify-md-center" py-0 height="100">
          <p py-0 class="text-align:center">
            <strong>Kontaktujte nás:</strong> grilovacka@seznam.cz |
            &copy; 2020
            <strong>Radka a Markéta</strong>
          </p>
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
      currentLatLng: latLng(49.1884422, 16.6147289),// zatím czechitas house
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
          grillImage: require("./assets/photos/00_Luzanky1.jpeg"),
          palivoInfo: 'gril je elektrický',
          wcInfo:'',
          parkingInfo: "parkoviště 100 m",
          reservationInfo:`Rezervujte zde: <a target="_blank" href="http://publicgrills.com/">publicgrills</a >. Maximální doba jedné rezervace jsou 2 hodiny.`,
          chargeInfo:'Poplatek za jednu rezervaci je 20Kč (prostřednictvím SMS)',
          sportsGroundInfo: " venkovní posilovna",
          playgroundInfo: " pískoviště, houpačky",
          mhd:'pionýrská (tram: 1,2,4,6,8,10,12;trolejbus: 25,26,38,39); zimní stadion(bus:67)',
          zajimavost:'',
          comments: [
            { date: "20.6.", text: "bylo to hezke misto" },
            { date: "20.5.", text: "hrozne" }
          ],
          distance:
            Math.round(
              latLng(49.2079947, 16.6066672).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 1,
          name: "Veřejný gril Špilberk",
          position: latLng(49.193722, 16.601805),
          type: "grill",
          rating: [3, 4, 3],
          wc: true,
          parking: true,
          reservation: true,
          charge: true,
          sportsGround: false,
          playground: false,
          grillImage: require("./assets/photos/01_spielberk1.jpg"),
          palivoInfo: 'gril je elektrický',
          wcInfo:'',
          parkingInfo: "parkoviště 500 m - Domini park",
          reservationInfo:`Rezervujte zde: <a target="_blank" href="http://publicgrills.com/">publicgrills</a >. Maximální doba jedné rezervace jsou 2 hodiny.`,
          chargeInfo:'Poplatek za jednu rezervaci je 20Kč (prostřednictvím SMS)',          
          sportsGroundInfo: "",
          playgroundInfo: '',
          mhd:'šilingrovo náměstí (tram: 1,3,4,5,6,12)',
          zajimavost:'hrad Špilberk',
          comments: [  
          ],
          distance:
            Math.round(
              latLng(49.193722, 16.601805).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },

        {
          id: 2,
          name: "Veřejný gril Kraví hora",
          position: latLng(49.2035717, 16.5842714),
          type: "grill",
          rating: [3, 4, 3],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true,
          grillImage: require("./assets/photos/02_kraviHora1.jpg"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "",
          parkingInfo: "200 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "",
          playgroundInfo: "prolézačky, klouzačky, houpačky",
          mhd: "náměstí Míru (tram:4;  bus: 68)",
          zajimavost:'planetárium',
          comments: [],
          distance:
            Math.round(
              latLng(49.2035717, 16.5842714).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 3,//neúplná data
          name: "Nový Lískovec",
          position: latLng(49.1754453, 16.5487825),
          type: "grill",
          rating: [3, 4, 3],
          wc: true,
          parking: true,
          reservation: true,
          charge: true,
          sportsGround: false,
          playground: false,
          grillImage: require("./assets/photos/03_novyLiskovec3jpg.jpg"),
          palivoInfo: " gril je elektrický",
          wcInfo: "",
          parkingInfo: "parkoviště 500 m - Domini park",
          reservationInfo: ` Maximální doba jedné rezervace jsou 2 hodiny; rezervujte zde: http://publicgrills.com/`,
          chargeInfo:
            "Poplatek za jednu rezervaci je 20Kč (prostřednictvím SMS)",
          sportsGroundInfo: "",
          playgroundInfo: "",
          mhd: "šilingrovo náměstí (tram: 1,3,4,5,6,12)",
          zajimavost:'',
          comments: [],
          distance:
            Math.round(
              latLng(49.1754453, 16.5487825).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 4,
          name: "Kozí horka",
          position: latLng(49.2386636, 16.5052903),
          type: "grill",
          rating: [5, 5, 1],
          wc: false,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true,
          //grillImage: require("./assets/photos/kozihorka.heic"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "",
          parkingInfo: "90 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "hřiště na košíkovou 200 m",
          playgroundInfo: "pískoviště",
          mhd: "náměstí Míru (tram:4;  bus: 68)",
          zajimavost:'Brněnská přehrada',
          comments: [],
          distance:
            Math.round(
              latLng(49.2386636, 16.5052903).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 5,
          name: "pod Dymou I.",
          position: latLng(49.243622, 16.498543),
          type: "grill",
          rating: [5, 5, 5],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: true,
        //  grillImage: require("./assets/photos/poddymouI.heic"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "toi toi",
          parkingInfo: "130 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "",
          playgroundInfo: "prolézačky, pískoviště",
          mhd: "náměstí Míru (tram:4;  bus: 68)",
          zajimavost:'Brněnská přehrada',
          comments: [],
          distance:
            Math.round(
              latLng(49.243623, 16.498559).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
         {
          id: 6,
          name: "pod Dymou II.",
          position: latLng(49.243623, 16.498584),
          type: "grill",
          rating: [5, 5, 5],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: true,
         // grillImage: require("./assets/photos/poddymouII.heic"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "toi toi",
          parkingInfo: "130 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "",
          playgroundInfo: "prolézačky, pískoviště",
          mhd: "náměstí Míru (tram:4;  bus: 68)",
          zajimavost:'Brněnská přehrada',
          comments: [],
          distance:
            Math.round(
              latLng(49.2430192, 16.4991761).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 7,
          name: "Rokle",
          position: latLng(49.2480253, 16.4937339),
          type: "grill",
          rating: [1, 4, 3],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true,
         // grillImage: require("./assets/photos/rokle.heic"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "toi toi",
          parkingInfo: "100 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "pinpongový stůl, půjčovna paddleboardů",
          playgroundInfo: "prolézačky, pískoviště - 100 m",
          mhd: "zastávka Rokle 300 m (bus: 303)",
          zajimavost:'Brněnská přehrada; hrad Veveří 3,5 km',
          comments: [],
          distance:
            Math.round(
              latLng(49.2480253, 16.4937339).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 8,//neúplná data
          name: "Maloměřice",
          position: latLng(49.2270731, 16.6435522),
          type: "grill",
          rating: [1, 1, 3],
          wc: false,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: false,
        //  grillImage: require(""),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "",
          parkingInfo: "200 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "fitpark sluneční lázně",
          playgroundInfo: "",
          mhd: "zastávka Obřanský most 400 m (tram:4)",
          zajimavost:'',
          comments: [],
          distance:
            Math.round(
              latLng(49.2270731, 16.6435522).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 9,
          name: "Rakovec",
          position: latLng(49.2275075, 16.5104231),
          type: "grill",
          rating: [1, 2, 3],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true,
         // grillImage: require("./assets/photos/rakovec.heic"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "toi toi",
          parkingInfo: "130 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "venkovní posilovna, pinpongové stoly",
          playgroundInfo: "trampolína, prolézačky, houpací kůň",
          mhd: "zastávka Rakovec 150 m (bus: 303)",
          zajimavost:'Brněnská přehrada, občerstvení Rybářská bašta 130 m',
          comments: [],
          distance:
            Math.round(
              latLng(49.2275075, 16.5104231).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 10,//neúplná data
          name: "Líšeňská Rokle",
          position: latLng(49.2083800, 16.6796433),
          type: "grill",
          rating: [1, 2, 3],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true,
         // grillImage: require(""),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "toi toi",
          parkingInfo: "130 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "venkovní posilovna, pinpongové stoly",
          playgroundInfo: "prolézačky,",
          mhd: "",
          zajimavost:'',
          comments: [],
          distance:
            Math.round(
              latLng(49.2083800, 16.6796433).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 11,//neúplná data
          name: "Vyhlídka",
          position: latLng(49.223711, 16.633599),
          type: "grill",
          rating: [1, 2, 3],
          wc: true,
          parking: true,
          reservation: false,
          charge: false,
          sportsGround: true,
          playground: true,
         // grillImage: require(""),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "toi toi",
          parkingInfo: "130 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "venkovní posilovna, basketbalové hřiště",
          playgroundInfo: "houpačka, dětská lanovka, prolézačky,",
          mhd: "",
          zajimavost:'',
          comments: [],
          distance:
            Math.round(
              latLng(49.223711, 16.633599).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
        {
          id: 12,
          name: "Sokolské Koupaliště",
          position: latLng(49.244144, 16.508333),
          type: "grill",
          rating: [1, 2, 3],
          wc: false,
          parking: false,
          reservation: false,
          charge: false,
          sportsGround: false,
          playground: false,
         // grillImage: require("./assets/photos/sokolskekoupaliste.heic"),
          palivoInfo: "uhlí, dřevo, brikety",
          wcInfo: "",
          parkingInfo: "600 m",
          reservationInfo:'',
          chargeInfo:"",
          sportsGroundInfo: "",
          playgroundInfo: "",
          mhd: "",
          zajimavost:'Brněnská přehrada, lodní doprava',
          comments: [],
          distance:
            Math.round(
              latLng(49.2275075, 16.5104231).distanceTo(
                latLng(49.1884422, 16.6147289)
              ) / 100
            ) / 10
        },
      ]
    };
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Amatic+SC&family=Roboto&display=swap");

.reset{
letter-spacing: 3px;
}
.filterButton {
  background: black;
}

.footer {
  background: black;
}

.v-btn-toggle > .v-btn.v-btn--active {
  color: black !important;
}

.logo {
  margin: 0px 40px;
  vertical-align: middle;
}
.logoText {
  font-size: 60px;
  color: #ca0101;
  margin-left: 5px;
  vertical-align: middle;
}

.v-app-bar--fixed {
  z-index: 10000000 !important;
}

.menu-wrapper {
  background: #393939;
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