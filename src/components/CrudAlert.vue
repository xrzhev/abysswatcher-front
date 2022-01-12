<template>
  <div v-if="issuccess === true">
  <v-alert :value="isAlert" type="success" transition="fade-transition">
    Action Successful
  </v-alert>
  </div>
  <div v-else>
    <v-alert :value="isAlert" type="error" transition="fade-transition">
      Action failure
    </v-alert>
  </div>
</template>

<script>
  export default {
    name: 'CrudAlert',
    data () {
      return {
        isSuccess: true,
        close_time: 3,
        isAlert: false,
      }
    },
    methods: {
      openAlert(isSuccess) {
        this.isSuccess = isSuccess
        this.isAlert = true
      },
      closeAlert() {
        this.isAlert = false
      }
    },
    watch: {
      isAlert(newVal) {
        if (newVal === "true") {
          setTimeout(()=>{this.closeAlert()}, 3000)
        }
      }
    }
  }
</script>