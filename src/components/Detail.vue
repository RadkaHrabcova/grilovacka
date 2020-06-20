  <template>
  <div>
    <v-row class="allName">
      <img v-bind:src="require('../assets/icons/grill.svg')" width="40" height="40" />
      <p class="name">{{oneGrill.name}}</p>

      <p>hodnocení: {{average(oneGrill.rating)}}</p>
      <v-btn target="_blank" :href="createLink(oneGrill.position)">naviguj</v-btn>
    </v-row>
    <img v-bind:src="oneGrill.grillImage" width="100%" />
  </div>
</template>
      
      <script>
export default {
  props: ["oneGrill"],

  methods: {
    createLink(position) {
      console.log(position);
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
}
</style>
