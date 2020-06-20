<template>
  <div style="padding-left: 25px">
    <v-text-field v-model="search" label="Vyhledat místo" solo @input="searchGrills"></v-text-field>

    <v-data-table
      :headers="headers"
      :items="searchedGrills"
      :sort-by="['distance', 'rating']"
      :sort-desc="[false, true]"
      multi-sort
      class="elevation-1"
    ></v-data-table>
  </div>
</template>

<script>
export default {
  methods: {
    searchGrills(data) {
      this.$emit("search-grills", data);
    }
  },

  props: ["searchedGrills"],

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
      ]
    };
  }
};
</script>

<style>
#keep .v-navigation-drawer__border {
  display: none;
}
</style>