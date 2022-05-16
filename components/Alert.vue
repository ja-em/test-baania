<template>
  <v-dialog v-model="dialog" width="250" persistent>
    <v-card class="pa-5">
      <v-container>
        <v-row justify="center"
          ><v-icon size="60" :color="alertType.toLowerCase()"
            >{{
              alertType === 'Success' ? 'mdi-close-circle' : 'mdi-check-circle'
            }}
          </v-icon></v-row
        >

        <v-row justify="center" class="font-weight-bold">{{ alertType }}</v-row>
        <v-row justify="center">{{ alertMessage }}</v-row>
        <v-row justify="center" class="mt-5">
          <v-btn color="grey" outlined @click="dialog = false">
            {{ alertType === 'Success' ? 'CONTINUE' : 'TRY AGAIN' }}
          </v-btn>
        </v-row>
      </v-container>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'AlertComponent',
  props: {
    alertType: {
      type: String,
      default: 'Success',
    },
    alertMessage: {
      type: String,
      default: 'Create a Successfully',
    },
  },
  data() {
    return {
      dialog: false,
    }
  },
}
</script>
