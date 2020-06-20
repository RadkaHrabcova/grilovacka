<template>
  <div style="padding-left: 25px">
    <v-text-field label="Vyhledat místo" solo @input="searchGrills"></v-text-field>

    <v-data-table
      @click:row="selectGrill"
      :headers="headers"
      :items="searchedGrills"
      :sort-by="['distance', 'rating']"
      :sort-desc="[false, true]"
      multi-sort
      class="elevation-1"
    >
      <template v-slot:item.rating="{ item }">{{average(item.rating)}}</template>

      <template v-slot:item.position="{ item }">
        <v-btn target="_blank" :href="createLink(item.position)">naviguj</v-btn>
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
        {
          text: this.searchedGrills.length + " výsledků",
          align: "start",
          sortable: false,
          value: "name"
        },

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

<style>
#keep .v-navigation-drawer__border {
  display: none;
}
</style>