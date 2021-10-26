<template>
  <section>
    <b-button
      label="Launch component modal"
      type="is-primary"
      size="is-medium"
      @click="isModalActive = true" />

    <b-modal
      v-model="isModalActive"
      has-modal-card
      :destroy-on-hide="false"
      aria-role="dialog"
      aria-label="Add Host"
      aria-modal>
        <form action="">
          <div class="modal-card" style="width: auto">
              <header class="modal-card-head">
                  <p class="modal-card-title">Add Host</p>
                  <button
                      type="button"
                      class="delete"
                      @click="changeModal"/>
              </header>
              <section class="modal-card-body">
                  <b-field label="HostName">
                      <b-input
                          v-model="hostname"
                          placeholder="www.example.com"
                          required>
                      </b-input>
                  </b-field>
                  <b-field label="URL">
                      <b-input
                          v-model="hosturl"
                          placeholder="https://www.example.com"
                          required>
                      </b-input>
                  </b-field>
                  <b-field label="Port" v-for="(port, index) in ports" :key="index">
                      <b-input
                          v-model="ports[index]"
                          placeholder=443
                          required>
                      </b-input>
                      <b-button icon-left="plus-thick" @click="addPort"/>
                      <b-button icon-left="minus-thick" @click="removePort" :disabled="isArrayIndexZero(index)"/>
                  </b-field>

              </section>
              <footer class="modal-card-foot">
                {{api_response}}
                  <b-button
                      label="Submit"
                      type="is-primary"
                      @click="submitHost" />
              </footer>
          </div>
      </form>
    </b-modal>
  </section>
</template>

<script>
export default {
  name: 'AddHostModal',
  data() {
    return {
      isModalActive: false,
      hostname: "",
      hosturl: "",
      ports: ["443"],
      api_response: ""
    }
  },
  methods: {
    changeModal: function() {
      this.isModalActive = false
    },
    addPort: function() {
      this.ports.push(null)
    },
    removePort: function() {
      this.ports.pop()
    },
    isArrayIndexZero: function(index) {
      if( index === 0 )  {
        return true
      } else {
        return false
      }
    },
    submitHost: function() {
      fetch("http://localhost:8000"+ "/set/host", {
        method: "POST",
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify( {"name":this.hostname, "url":this.hosturl, "ports": this.ports} )
      }
        ).then(resp=>{
            return resp.json()
        }).then(text=>{
          this.api_response = text
        }).catch(err=>{
          console.log(err)
        })
    }


  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
