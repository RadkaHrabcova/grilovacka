  <template>
  <div>
    <v-row class="allName">
      <img v-bind:src="require('../assets/icons/grill.svg')" width="50" height="50" />
      <p class="name">{{oneGrill.name}}</p>

      <p>hodnocení: {{average(oneGrill.rating)}}</p>
      <v-btn target="_blank" :href="createLink(oneGrill.position)">naviguj</v-btn>
      <v-btn icon>
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-row>
    <img v-bind:src="oneGrill.grillImage" width="500" />

    <div class="detailInfo">
      <ul>
        <li v-if="oneGrill.parkingInfo">
          <img v-bind:src="require('../assets/icons/parking.svg')" width="40" height="40" />
          {{oneGrill.parkingInfo}}
        </li>
        <li>
          <img v-bind:src="require('../assets/icons/grill.svg')" width="40" height="40" />
          {{oneGrill.palivoInfo}}
        </li>
        <li>
          <img v-bind:src="require('../assets/icons/playground.svg')" width="40" height="40" />
          {{oneGrill.playgroundInfo}}
        </li>

        <li>
          <img v-bind:src="require('../assets/icons/football.svg')" width="40" height="40" />
          {{oneGrill.sportsGroundInfo}}
        </li>
      </ul>
    </div>
  </div>
</template>
      
      <script>
export default {
  props: ["oneGrill"],

  methods: {
    createLink(position) {
      return `https://maps.google.com/?ll=${position.lat},${position.lng}`;
    },

    average(rating) {
      return Math.round(rating.reduce((a, b) => a + b) / rating.length);
    }
  }
};
</script>
      
<style>
.allName {
   padding: 20px;
  justify-content: space-between;
  vertical-align: middle;
}

.detailInfo {
  padding: 50px;
  
}

.detailInfo ul{
list-style: none;
}

.detailInfo li{
  margin-bottom: 10px;
}
.detailInfo img{
  vertical-align: middle;
  margin-right: 15px;
}
</style>
