<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer/>

      <v-btn @click="addCert" class="mx-3" outlined>
        <v-icon>mdi-clipboard-plus-outline</v-icon>
        <span>Add Cert</span>
      </v-btn>
      <v-btn @click="checkCerts" class="" outlined>
        <v-icon>mdi-clipboard-pulse-outline</v-icon>
        <span>All check cert</span>
      </v-btn>
    </v-app-bar>

    <v-main>
      <CertTable :api_url='api_url' ref="certtable"/>
    </v-main>
  </v-app>
</template>

<script>
import CertTable from './components/CertTable.vue';

export default {
  name: 'App',

  components: {
    CertTable,
  },
  methods: {
    addCert() {
      this.$refs.certtable.addCert()
    },
    checkCerts() {
      this.$refs.certtable.checkCerts()
    }
  },
  data: () => ({
    api_url: "http://192.168.11.199:8000"
  }),
};
</script>
