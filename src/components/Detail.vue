  <template>
  <div>
    <v-row no-gutters class="allName align-center text-left">
      <v-btn icon @click="closeDetail">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>

      <div style="flex:1;" class="pl-3">
        <p class="name py-0 my-0">{{oneGrill.name}}</p>
        <v-rating
          small
          dense
          color="orange"
          class="mr-1"
          empty-icon
          :value="average(oneGrill.rating)"
        ></v-rating>
      </div>

      <v-btn icon target="_blank" :href="createLink(oneGrill.position)">
        <v-row class="justify-center align-center">
          <img class="smaller mt-1" :src="require('../assets/icons/navigate.svg')" />
        </v-row>
      </v-btn>
    </v-row>
    <img v-bind:src="oneGrill.grillImage" width="500" />

    <div class="detailInfo">
      <ul>
        <li v-if="oneGrill.parkingInfo">
          <img v-bind:src="require('../assets/icons/parking.svg')"  />
          {{oneGrill.parkingInfo}}
        </li>
        <li v-if="oneGrill.palivoInfo">
          <img v-bind:src="require('../assets/icons/grill.svg')" />
          {{oneGrill.palivoInfo}}
        </li>
        <li v-if="oneGrill.playgroudInfo">
          <img v-bind:src="require('../assets/icons/playground.svg')" />
          {{oneGrill.playgroundInfo}}
        </li>

        <li v-if="oneGrill.sportsGroundInfo">
          <img v-bind:src="require('../assets/icons/football.svg')" />
          {{oneGrill.sportsGroundInfo}}
        </li>
      </ul>
    </div>

    <div class="komentar">
      <div v-if="oneGrill.comments && oneGrill.comments.length > 0">
        <div
          v-for="comment in oneGrill.comments"
          :key="comment.name"
        >{{comment.name}} - {{comment.text}}</div>
      </div>

      <div v-else>nejsou zadne komentare</div>

      <v-form>
        <v-text-field v-model="name" label="Name" required></v-text-field>

        <v-text-field v-model="text" label="text" required></v-text-field>

        <v-btn color="success" class="mr-4" @click="add" :disabled="!buttonEnabled">Přidej komentář</v-btn>
      </v-form>
    </div>
  </div>
</template>
      
      <script>
export default {
  props: ["oneGrill"],

  data() {
    return {
      name: "",
      text: ""
    };
  },

  computed: {
    buttonEnabled() {
      return this.name.length > 0 && this.text.length > 0;
    }
  },
  methods: {
    add() {
      this.oneGrill.comments.push({ name: this.name, text: this.text });
      this.name = "";
      this.text = "";
    },
    createLink(position) {
      return `https://maps.google.com/?ll=${position.lat},${position.lng}`;
    },

    average(rating) {
      return Math.round(rating.reduce((a, b) => a + b) / rating.length);
    },
    closeDetail() {
      this.$emit("closeDetail");
    }
  }
};
</script>
      
<style>
.komentar {
  padding-left: 20px;
}
.allName {
  padding: 20px;
  justify-content: space-between;
  vertical-align: middle;
}

.detailInfo {
  padding: 50px 10px;
}

.detailInfo ul {
  list-style: none;
}

.detailInfo li {
  margin-bottom: 10px;
}
.detailInfo img {
  width: 30px;
  height: 30px;
  vertical-align: middle;
  margin-right: 15px;
}

.smaller {
  max-width: 24px;
  max-height: 24px;
}
</style>
