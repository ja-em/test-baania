<template>
  <v-dialog v-model="dialog" width="500" persistent>
    <v-card class="rounded-md pa-5">
      <v-container>
        <v-row class="font-weight-bold pl-4">Create</v-row>
        <v-form ref="form" v-model="valid">
          <v-row>
            <v-col cols="6">
              <v-text-field
                v-model.trim="name"
                outlined
                dense
                hide-details
                label="Name"
                placeholder="Name"
                :rules="[(v) => !!v || 'Name is required']"
              ></v-text-field>
            </v-col>
            <v-col cols="3">
              <v-text-field
                v-model.trim="postCode"
                outlined
                dense
                hide-details
                label="Post Code"
                placeholder="Post Code"
                :rules="[(v) => !!v || 'Post Code is required']"
              ></v-text-field>
            </v-col>
            <v-col cols="3">
              <v-text-field
                v-model="price"
                outlined
                type="number"
                dense
                hide-details
                label="Price"
                placeholder="Price"
                :rules="[(v) => !!v || 'Price is required']"
              ></v-text-field>
            </v-col>
            <v-col>
              <v-textarea
                v-model.trim="description"
                outlined
                label="Description"
                hide-details
                :rules="[(v) => !!v || 'Description is required']"
              ></v-textarea>
            </v-col>
          </v-row>
        </v-form>
        <v-row justify="center" no-gutters class="mt-2">
          <v-btn outlined color="grey" class="mr-1" @click="dialog = false"
            >CANCEL</v-btn
          >
          <v-btn
            color="success"
            class="ml-1"
            :disabled="!valid"
            :loading="loading"
            @click="manageData"
            >CREATE</v-btn
          >
        </v-row>
      </v-container>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'CreateUpdate',
  inject: ['getBaseUrl', 'getPort'],
  props: {
    dialogType: {
      type: String,
      default: 'create',
    },
  },
  data() {
    return {
      dialog: false,
      valid: true,
      loading: false,
      name: '',
      postCode: '',
      price: '',
      description: '',
    }
  },
  computed: {
    baseUrl() {
      return `${this.getBaseUrl()}${
        this.getPort() === 80 ? '' : ':' + this.getPort()
      }`
    },
  },
  methods: {
    manageData() {
      if (this.dialogType === 'create') {
        this.createData()
      }
      // else {
      //     this.updateData()
      //   }
    },
    async createData() {
      try {
        this.loading = true
        const obj = {
          name: this.name,
          desc: this.description,
          post_code: this.postCode,
          price: this.price,
        }
        console.log(obj)
        await this.$axios.$post(`${this.baseUrl}/home`, obj)
        this.$refs.form.reset()
        this.$refs.form.resetValidation()
        this.loading = false
        this.dialog = false
      } catch (e) {
        this.loading = false
        console.log(e)
      }
    },
  },
}
</script>
