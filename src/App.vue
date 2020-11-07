<template>
  <div class="columns" id="app">
    <div class="right">
      <SortableList />
    </div>
    <div class="left">
      <ComponentsList />
    </div>
  </div>
</template>

<script>
import SortableList from './components/SortableList.vue'
import ComponentsList from './components/ComponentsList.vue'

export default {
  name: 'App',
  components: {
    SortableList,
    ComponentsList
  }
}
</script>

<style scoped>
* {
  box-sizing: content-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin: 30px auto;
  width: min(1080px, 100%);
}

.columns {
  display: flex;
}

.columns div {
  margin: 0 1em;
}

.columns .right {
  flex: 1 0;
}
</style>
