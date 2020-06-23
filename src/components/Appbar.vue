<template>
  <div class="banner">
    <v-app-bar>
      <v-col md="4">
        <h2>Grilovačka</h2>
      </v-col>

      <v-col md="6">
        <v-row>
          <v-btn class="mx-2" fab color="indigo">
            <v-icon>mdi-plus</v-icon>
          </v-btn>
          <v-btn class="mx-2" fab dark color="orange">
            <v-icon dark>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn class="mx-2" fab color="teal">
            <v-icon mdi-Parking></v-icon>
          </v-btn>
          <v-btn class="mx-2" fab dark color="blue">
            <v-icon dark>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn class="mx-2" fab dark color="green">
            <v-icon dark>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn class="mx-2" fab dark color="yellow">
            <v-icon dark>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn class="mx-2" fab dark color="pink">
            <v-icon dark>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn class="mx-2" fab dark color="brown">
            <v-icon dark>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
        </v-row>
      </v-col>

      <v-col md="2">
        <v-row>
          <v-btn class="ma-2" tile outlined color="success">
            <h2>Zobrazit</h2>
          </v-btn>

          <v-btn class="ma-2" tile outlined color="success">
            <h2>Resetovat vše</h2>
          </v-btn>
        </v-row>
      </v-col>
    </v-app-bar>
  </div>
</template>

<script>
export default {};
</script>

<style>
</style>