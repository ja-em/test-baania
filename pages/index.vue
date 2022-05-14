<template>
  <div>
    <v-row class="head-color pa-10"  >
      <v-col cols="1"></v-col>
      <v-col cols="12" md="4" align-self="end">
        <v-text-field outlined dense hide-details placeholder="https://test-backend.baania.dev"></v-text-field>
      </v-col>
      <v-col cols="12" md="4" align-self="end">
        <v-text-field outlined dense hide-details placeholder="80"></v-text-field>
      </v-col>
      <v-col cols="12" md="2" align-self="end" align="center">
        <v-btn color="primary">CONNECT</v-btn>
      </v-col>
       <v-col cols="1"></v-col>
    </v-row>
    <v-row class="px-10">
    <v-col cols="1"></v-col>
    <v-col cols="2" class="font-weight-bold">HOUSE LIST</v-col>
    <v-col cols="6"></v-col>
    <v-col cols="2"> <v-btn color="success">CONNECT</v-btn></v-col>
    <v-col cols="1"></v-col>
    </v-row>
  </div>
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
