<template>
  <div id="app">
    <div class="header hero is-primary">
      <div class="hero-body">
        <p class="title has-text-centered">
          <b-icon icon="eye" size="is-medium"/>
          AbyssWatcher
          <b-icon icon="eye" size="is-medium"/>
        </p>
      </div>
    </div>
    <div class="columns is-centered">
      <div class="column is-10 mt-4">
        <Table />
      </div>
    </div>
    <AddHostModal />
  </div>
</template>

<script>
import Table from './components/Table.vue'
import AddHostModal from './components/AddHostModal.vue'

export default {
  name: 'App',
  components: {
    Table,
    AddHostModal,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
