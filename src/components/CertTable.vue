<template>
  <div v-if="isLoading">
    <v-data-table loading hide-default-footer/>
  </div>
  <div v-else>
    <CrudAlert ref="crudalert"/>
    <CrudModal :isOpen.sync="isOpenDialog" :mode="requestMode" :cert="apiTargetItem" :api_url="api_url" @reloadTable="getCertList" />
    <v-data-table
      class="certtable"
      item-key="id"
      :headers="table_headers"
      :items="certs"
      :items-per-page="15"
      :footer-props="{
        showFirstLastPage: true
      }"
    >
      <template v-slot:[`item.cert_state`]="{ item }">
        <v-chip :color="getColor(item.cert_state)">
          {{ item.cert_state }}
        </v-chip>
      </template>
      <template v-slot:[`item.domain`]="{ item }">
          <a :href="'https://' + item.domain" target="_blank" rel="noopener noreferrer">
            {{ item.domain }}
          </a>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon class="mr-2" @click="editCert(item)">mdi-pencil</v-icon>
        <v-icon class="mr-2" @click="deleteCert(item)">mdi-delete</v-icon>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  import CrudModal from './CrudModal.vue'
  import CrudAlert from './CrudAlert.vue'
  export default {

    name: 'CertTable',
    components: {
      CrudModal,
      CrudAlert
    },
    props: {
      api_url: {
        required: true
      }
    },
    data () {
      return {
        certs: null, 
        isLoading: true,
        isOpenDialog: false,
        apiRequestMethod: "",
        apiTargetItem: null, 
        table_headers: [
          {text: 'ID', value: 'id'},
          {text: 'State', value: 'cert_state'},
          {text: 'Domain', value: 'domain'},
          {text: 'Port', value: 'port'},
          {text: 'Begin time', value: 'begin_time'},
          {text: 'Expire time', value: 'expire_time'},
          {text: 'Issuer', value: 'issuer'},
          {text: 'Actions', value: 'actions', sortable: false},
        ]
      }
    },
    created: function(){
      this.getCertList()
    },
    methods: {
      getCertList() {
        fetch(this.api_url + '/certs')
        .then(resp => resp.json())
        .then((data) => {
          this.certs = data
          this.isLoading = false
        })
      },
      checkCerts() {
        this.isLoading = true
        fetch(this.api_url + '/certs/check',{method: 'POST'})
        .then(resp => resp.json())
        .then((data) => {
          console.log(data)
          this.getCertList()
        })
      },
      addCert() {
        this.isOpenDialog = true
        this.requestMode = "Add"
        this.apiTargetItem = null
      },
      editCert(cert) {
        this.isOpenDialog = true
        this.requestMode = "Edit"
        this.apiTargetItem = cert
      },
      deleteCert(cert) {
        this.isOpenDialog = true
        this.requestMode = "Delete"
        this.apiTargetItem = cert
      },
      getColor(cert_state) {
        if (cert_state == 'Fine') {
          return 'success'
        } else if (cert_state == 'Warning') {
          return 'warning'
        } else if (cert_state == 'Danger') {
          return 'error'
        } else if (cert_state == 'Expired') {
          return 'blue-gray'
        }
      },
      openAlert(isSuccess) {
        this.$refs.crudalert.openAlert(isSuccess)
      },
      closeAlert() {
        this.$refs.crudalert.closeAlert()
      }
    },
  }
</script>