<template>
<v-dialog v-model="isOpen" persistent max-width="600px">
  <v-card>
    <v-card-title class="text-h5 grey lighten-2">
      {{ mode }} Cert
    </v-card-title>
    <div v-if="mode !== 'Delete'">
      <v-card-text> 
        <v-container>
          <v-row>
            <v-col> 
              <v-text-field v-model="modify_domain" label="Domain">
              </v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="modify_port" label="Port">
              </v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          outlined
          @click="closeDialog"
        >
          Close
        </v-btn>
        <v-btn
          color="primary"
          :loading="isLoading"
          @click="callAPICrud"
        >
          Save
        </v-btn>
      </v-card-actions>
    </div>
    <div v-else>
      <v-card-text> 
        <v-container>
          <p class="text-body-1">
            Are you sure to DELETE <b>{{ cert.domain }}</b> from AbyssWatcher?
          </p>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          outlined
          @click="closeDialog"
        >
          Cancel
        </v-btn>
        <v-btn
          color="error"
          :loading="isLoading"
          @click="callAPICrud"
        >
          DELETE
        </v-btn>
      </v-card-actions>
    </div>
  </v-card>
</v-dialog>
</template>

<script>
  export default {
    name: 'CrudModal',
    props: {
      isOpen: {
        required: true
      },
      mode: {
        required: true
      },
      cert: {
      },
      api_url: {
        required: true
      }
    },
    data () {
      return {
        isLoading: false,
        modify_domain: null,
        modify_port: null,
        api_call_url: null,
      }
    },
    methods: {
      callAPICrud() {
        this.isLoading = true
        this.api_crud_url = (this.cert === null ? this.api_url + '/cert' : this.api_url + '/cert/' + this.cert.id)
        fetch(this.api_crud_url , 
        {
          method: this.mode2Method(this.mode),
          mode: 'cors',
          cache: 'no-cache',
          headers: {'Content-Type':'application/json'},
          body: JSON.stringify({'domain': this.modify_domain, 'port': this.modify_port})
        })
        .then(resp => resp.json())
        .then((data) => {
          this.isLoading = false
          this.$emit("reloadTable")
          console.log(data)
        })
      },
      closeDialog() {
        this.isOpen = false
        this.$emit("update:isOpen", this.isOpen)
      },
      mode2Method(mode) {
        if (mode === "Add") {
          return "POST"
        } else if (mode === "Edit") {
          return "PUT"
        } else if (mode === "Delete") {
          return "DELETE"
        }
      }
    },
    watch: {
      isOpen: function() {
        if (this.cert !== null) {
          this.modify_domain = this.cert.domain
          this.modify_port = this.cert.port
        } else {
          this.modify_domain = ""
          this.modify_port = 443
        }
      }
    }
  }
</script>