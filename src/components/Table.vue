<template>
  <b-table :data="rows"
           :hoverable="true"
           :paginated="true"
           :per-page="15"
           :row-class="rowColorSelector">
      <b-table-column label="ID" field="id" width="80" numeric sortable v-slot="props">
        {{ props.row.id }}
      </b-table-column>
      <b-table-column label="Status" field="state" width="100" sortable v-slot="props">
        <b-tag :type="tagColorSelector(props.row)">
          {{ props.row.state }}
        </b-tag>
      </b-table-column>
      <b-table-column label="HostName" field="name" sortable searchable v-slot="props">
        {{ props.row.name }}
      </b-table-column>
      <b-table-column label="URL" field="url" sortable searchable v-slot="props">
        <a :href="props.row.url">{{ props.row.url }}</a>
      </b-table-column>
      <b-table-column label="Port" field="port" sortable numeric v-slot="props">
        {{ props.row.port }}
      </b-table-column>
      <b-table-column label="Cert Begin Date" field="begin_date" sortable v-slot="props">
        {{ props.row.begin_date }}
      </b-table-column>
      <b-table-column label="Cert Expire Date" field="expire_date" sortable v-slot="props">
        {{ props.row.expire_date }}
      </b-table-column>
      <b-table-column label="Issuer" field="issuer" sortable v-slot="props">
        {{ props.row.issuer}}
      </b-table-column>
  </b-table>
</template>

<script>
export default {
  name: 'Table',
  data: function() {
    return {
      // api_base: location.protocol + "//" + location.hostname + ":8000"
      api_base: "http://localhost:8000",
      rows: "",
    }
  },
  methods: {
    rowColorSelector: function(row) {
      if (row.state === "Fine") {
        return "is-fine"
      } else if (row.state === "Warning") {
        return "is-warning"
      } else if (row.state === "Danger") {
        return "is-danger"
      } else if (row.state === "Expired") {
        return "is-expired"
      }
    },
    tagColorSelector: function(row) {
      if (row.state === "Fine") {
        return "is-success"
      } else if (row.state === "Warning") {
        return "is-warning"
      } else if (row.state === "Danger") {
        return "is-danger"
      } else if (row.state === "Expired") {
        return "is-black"
      }
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
<style>
tr.is-fine {
  background: #EFFAF3;
}
tr.is-warning {
  background: #FFF0B3;
}
tr.is-danger {
  background: #FFCCD7;
}
tr.is-expired {
  background: #363636;
  color: #DB143C;
}
</style>
