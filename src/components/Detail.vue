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

    <v-carousel cycle height="350" hide-delimiter-background show-arrows-on-hover>
      <v-carousel-item v-for="(onePicture, i) in oneGrill.grillImage" :key="i">
        <img v-bind:src="onePicture" width="100%" />
      </v-carousel-item>
    </v-carousel>

    <div class="detailInfo">
      <ul>
        <li v-if="oneGrill.palivoInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/grill.svg')" />
            {{oneGrill.palivoInfo}}
          </div>
        </li>

        <li v-if="oneGrill.reservationInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/reservation.svg')" />
            <p v-html="oneGrill.reservationInfo"></p>
          </div>
        </li>
        <li v-if="oneGrill.chargeInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/dollar-sign.svg')" />
            {{oneGrill.chargeInfo}}
          </div>
        </li>
        <li v-if="oneGrill.mhd">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/mhd.png')" />
            {{oneGrill.mhd}}
          </div>
        </li>
        <li v-if="oneGrill.parkingInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/parking.svg')" />
            {{oneGrill.parkingInfo}}
          </div>
        </li>
        <li v-if="oneGrill.playgroundInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/playground.svg')" />
            {{oneGrill.playgroundInfo}}
          </div>
        </li>
        <li v-if="oneGrill.sportsGroundInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/field.svg')" />
            {{oneGrill.sportsGroundInfo}}
          </div>
        </li>
        <li v-if="oneGrill.wcInfo">
          <div class="d-flex">
            <img v-bind:src="require('../assets/icons/wc-sign.svg')" />
            {{oneGrill.wcInfo}}
          </div>
        </li>
      </ul>
    </div>
    <div v-if="oneGrill.zajimavost" class="zajimavost">
      <h2>Turistická zajímavost</h2>

      <P>{{oneGrill.zajimavost}}</P>
    </div>
    <div class="komentar">
      <h2>Komentáře</h2>
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
.komentar,
.zajimavost {
  padding: 0 40px;
}

.zajimavost h2,
.komentar h2 {
  letter-spacing: 2px;
  padding-bottom: 30px;
  text-align: center;
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
