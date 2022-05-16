<template>
  <div>
    <v-row class="head-color pa-10">
      <v-col cols="1"></v-col>
      <v-col cols="12" md="4" align-self="end">
        <v-text-field
          v-model="baseUrl"
          :disabled="connectType"
          outlined
          dense
          hide-details
          placeholder="https://test-backend.baania.dev"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="4" align-self="end">
        <v-text-field
          v-model="port"
          :disabled="connectType"
          outlined
          dense
          hide-details
          placeholder="80"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="2" align-self="end" align="center">
        <v-btn
          color="primary"
          :loading="connectLoading"
          @click="connectTypeMethod"
        >
          {{ connectType ? 'DISCONNECT' : 'CONNECT' }}</v-btn
        >
      </v-col>
      <v-col cols="1"></v-col>
    </v-row>
    <v-row class="px-10">
      <v-col cols="1"></v-col>
      <v-col cols="4" md="2" class="font-weight-bold">HOUSE LIST</v-col>
      <v-col cols="1" md="6"></v-col>
      <v-col cols="4" md="2" align="center">
        <v-btn
          color="success"
          :disabled="!connectType"
          @click="openDialog('create')"
        >
          CREATE</v-btn
        ></v-col
      >
      <v-col cols="1"></v-col>
    </v-row>
    <v-row class="px-10">
      <v-col cols="1"></v-col>
      <v-col>
        <v-data-table
          :headers="headers"
          :items="payload"
          :items-per-page="5"
          class="table"
        >
          <template #[`item.actions`]="{ item }">
            <v-container>
              <v-row justify="center">
                <v-btn
                  rounded
                  small
                  class="btn-detail mr-1"
                  @click="openDialog('update', item)"
                  >VIEW DETAIL</v-btn
                >
                <v-btn
                  rounded
                  small
                  :loading="item.loading"
                  class="btn-delete ml-1"
                  @click="deleteData(item)"
                  >DELETE</v-btn
                >
              </v-row>
            </v-container>
          </template>
        </v-data-table>
      </v-col>
      <v-col cols="1"></v-col>
    </v-row>
    <v-row>
      <v-col cols="1"></v-col>
      <v-col style="background-color: #f4f7fc" class="py-10">
        <v-row justify="center"
          ><div style="width: 30%">
            <v-autocomplete
              v-model="selectPostCode"
              outlined
              dense
              :disabled="!connectType"
              :items="payload"
              item-text="post_code"
              label="SELECT POST CODE"
              hide-details
              @change="getPostCodeDetail"
            ></v-autocomplete></div
        ></v-row>
        <v-row
          v-if="selectPostCode !== null"
          justify="center"
          class="primary--text mt-5"
          ><div style="width: 30%">Average : {{ average }}</div></v-row
        >
        <v-row
          v-if="selectPostCode !== null"
          justify="center"
          class="primary--text mt-5"
          ><div style="width: 30%">Median : {{ median }}</div></v-row
        >
      </v-col>
      <v-col cols="1"></v-col>
    </v-row>
    <CreateUpdate ref="createUpdate" @connectData="connectData" />
    <Alert ref="alert" :alert-type="alertType" :alert-message="alertMessage" />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  provide() {
    return {
      getBaseUrl: () => this.baseUrl,
      getPort: () => this.port,
    }
  },
  data() {
    return {
      headers: [
        {
          text: 'ID',
          align: 'center',
          sortable: false,
          value: 'id',
        },
        {
          text: 'Name',
          value: 'name',
          sortable: false,
          align: 'center',
        },
        {
          text: 'Post Code',
          value: 'post_code',
          sortable: false,
          align: 'center',
        },
        {
          text: 'Price',
          value: 'price',
          sortable: false,
          align: 'center',
        },
        {
          text: 'Actions',
          value: 'actions',
          sortable: false,
          align: 'center',
        },
      ],
      baseUrl: 'https://test-backend.baania.dev',
      connectType: false,
      alertType: 'Success',
      alertMessage: '',
      port: 80,
      connectLoading: false,
      payload: [],
      median: null,
      average: null,
      selectPostCode: null,
    }
  },

  created() {},
  methods: {
    connectTypeMethod() {
      if (this.connectType) {
        this.disConnectData()
      } else {
        this.connectData()
      }
    },
    async connectData() {
      try {
        this.connectLoading = true
        const response = await this.$axios.$get(
          `${this.baseUrl}${
            this.port === 80 ? '' : ':' + this.port
          }/home?skip=1&take=100`
        )
        this.connectLoading = false
        // console.log(response)
        this.payload = response.payload.map((p) => ({ ...p, loading: false }))
        this.connectType = true
      } catch (e) {
        this.connectLoading = false
        console.error(e)
      }
    },
    disConnectData() {
      this.payload = []
      this.selectPostCode = null
      this.connectType = false
    },
    openDialog(type, item) {
      if (type === 'create') {
        this.$refs.createUpdate.setDialog(type, {
          desc: '',
          name: '',
          price: '',
          post_code: '',
          id: '',
        })
        this.$refs.createUpdate.dialog = true
      } else if (type === 'update') {
        // console.log(item)
        this.$refs.createUpdate.setDialog(type, item)
        this.$refs.createUpdate.dialog = true
      }
    },
    async deleteData(item) {
      try {
        item.loading = true
        await this.$axios.$delete(
          `${this.baseUrl}${this.port === 80 ? '' : ':' + this.port}/home/${
            item.id
          }`
        )
        this.connectData()
        this.alertType = 'Success'
        this.alertMessage = 'Delete a Successfull'
        this.$refs.alert.dialog = true
        item.loading = false
      } catch (e) {
        this.alertType = 'Fail'
        this.alertMessage = "Let's try one more again"
        this.$refs.alert.dialog = true
        item.loading = false
        console.error(e.response.data)
      }
    },
    async getPostCodeDetail() {
      try {
        const { payload } = await this.$axios.$get(
          `${this.baseUrl}${this.port === 80 ? '' : ':' + this.port}/postCode/${
            this.selectPostCode
          }`
        )
        // console.log(response)
        this.average = payload.average
        this.median = payload.median
      } catch (e) {
        console.error(e)
      }
    },
  },
}
</script>
<style scoped>
.head-color {
  background-color: #f4f7fc;
  height: 15%;
}
.table {
  width: 95%;
  border: solid 1px #000;
}
.btn-detail {
  background-color: #fff7e6 !important;
  color: #ff9900;
}
.btn-delete {
  background-color: #fdf4f7 !important;
  color: #b93e5c;
}
</style>
