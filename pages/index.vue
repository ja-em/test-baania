<template>
  <v-row class="head-color "  >
    <v-col cols="1"></v-col>
    <v-col cols="4" align-self="end">
      <v-text-field outlined dense hide-details placeholder="https://test-backend.baania.dev"></v-text-field>
    </v-col>
    <v-col cols="4" align-self="end">
      <v-text-field outlined dense hide-details placeholder="80"></v-text-field>
    </v-col>
    <v-col cols="2" align-self="end">
      <v-btn >CONNECT</v-btn>
    </v-col>
     <v-col cols="1"></v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
}
</script>
<style scoped>
.head-color{
  background-color: #F4F7FC;
  height: 15%;
 
}
</style>
