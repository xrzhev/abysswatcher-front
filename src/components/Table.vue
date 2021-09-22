<template>
  <b-table :columns="columns" :data="rows"
           :hoverable="true"
           :paginated="true"
           :per-page="15"
  />
</template>

<script>
export default {
  name: 'Table',
  data: function() {
    return {
      // api_base: location.protocol + "//" + location.hostname + ":8000"
      api_base: "http://localhost:8000",
      rows: "",
      columns:[
        {label: "ID", field: "id", width: "80", numeric: true, sortable: true},
        {label: "Status", field: "state", width: "100", sortable: true},
        {label: "HostName", field: "name", sortable: true, searchable: true},
        {label: "URL", field: "url", sortable: true, searchable: true},
        {label: "Port", field: "port", numeric: true, sortable: true},
        {label: "Cert Begin Date", field: "begin_date", sortable: true},
        {label: "Cert Expire Date", field: "expire_date", sortable: true},
        {label: "Issuer", field: "issuer", sortable: true},
      ]
    }
  },
  methods: {
    rowclickact: function(params) {
      alert(params.row.id)
    }
  },
  mounted: function() {
    fetch("http://localhost:8000"+ "/get"
      ).then(resp=>{
          return resp.json()
      }).then(text=>{
        this.rows = text
        console.log(this.rows)
      }).catch(err=>{
        console.log(err)
      })
  }
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
