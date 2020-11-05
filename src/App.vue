<template>
  <div class="columns">
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
