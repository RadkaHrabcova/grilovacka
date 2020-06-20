<template>
  <div style="padding-left: 15px">
    <v-text-field
      clearable
      prepend-inner-icon="mdi-magnify"
      hide-details
      label="Vyhledat místo"
      solo
      @change="searchGrills"
      @input="searchGrills"
    ></v-text-field>

    <v-data-table
      hide-default-footer
      @click:row="selectGrill"
      :headers="headers"
      :items="searchedGrills"
      :sort-by="['distance', 'rating']"
      :sort-desc="[false, true]"
      multi-sort
      class="elevation-1"
    >
      <template v-slot:item.image="{ item }">
        <v-row no-gutters class="justify-center align-center">
          <img
            class="image-wrapper"
            v-if="item.grillImage"
            :src="item.grillImage"
            alt="nahled obrazku"
          />
          <img
            class="image-wrapper"
            v-else
            src="https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcR8TKl-5A0GRek5ce4Hx4J0NU36J8dK10BNT2WRkaqC0G2HVcpc&usqp=CAU"
          />
        </v-row>
      </template>

      <template v-slot:item.rating="{ item }">
        <div class="title-name">
          <strong>{{ item.name }}</strong>
        </div>
        <v-rating small dense color="orange" empty-icon :value="average(item.rating)"></v-rating>
      </template>

      <template v-slot:item.position="{ item }">
        <v-btn icon target="_blank" :href="createLink(item.position)">
          <v-row class="justify-center align-center">
            <img class="smaller mt-1" :src="require('../assets/icons/navigate.svg')" />
          </v-row>
        </v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  methods: {
    createLink(position) {
      return `https://maps.google.com/?ll=${position.lat},${position.lng}`;
    },
    searchGrills(data) {
      this.$emit("search-grills", data);
    },

    average(rating) {
      return Math.round(rating.reduce((a, b) => a + b) / rating.length);
    },

    selectGrill(selectedGrill) {
      this.$emit("id", selectedGrill.id);
    }
  },

  computed: {
    headers() {
      return [
        { text: this.searchedGrills.length + " výsledků", value: "image" },

        { text: "Hodnocení", value: "rating" },
        { text: "Navigace", value: "position", sortable: false }
      ];
    }
  },

  props: ["searchedGrills"],

  data() {
    return {
      drawer: null
    };
  }
};
</script>

<style scoped>
.image-wrapper {
  max-width: 70px;
  height: 75px;
  object-fit: contain;
}

.smaller {
  max-width: 24px;
  max-height: 24px;
  width: 100%;
}

.title-name {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

#keep .v-navigation-drawer__border {
  display: none;
}
</style>