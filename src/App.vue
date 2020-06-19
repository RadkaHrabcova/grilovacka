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
        <v-text-field v-model="search" label="Vyhledat místo" solo></v-text-field>
        <v-app id="inspire">
          <v-data-table
            :headers="headers"
            :items="allGrills"
            :sort-by="['distance', 'rating']"
            :sort-desc="[false, true]"
            multi-sort
            class="elevation-1"
          ></v-data-table>
        </v-app>
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
export default {
  props: {
    source: String
  },
  data() {
    return {
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
          id: 2,
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
          id: 3,
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
      ]
    };
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Amatic+SC&family=Roboto&display=swap");

#keep .v-navigation-drawer__border {
  display: none;
}

body {
  font-family: "Roboto", sans-serif;
}
h1,
h2 {
  font-family: "Amatic SC", cursive;
}
</style>