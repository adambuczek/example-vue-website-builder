<template>
  <ul ref="list">
    <li
      v-for="(item, i) in listItems"
      :key="i"
    >
      {{ item }}<DragHandle />
    </li>
  </ul>
</template>

<script>
import Sortable from 'sortablejs'
import { h } from 'vue'

const DragHandle = {
  name: 'drag-handle',
  render: () => h('span', {
    class: 'drag-handle'
  }, '≡')
}

export default {
  name: 'SortableList',
  components: {
    DragHandle
  },
  data() {
    return {
      listItems: [
        'one',
        'two',
        'three',
        'four',
        'five'
      ]
    }
  },
  mounted () {
    const list = this.$refs.list
    Sortable.create(list, {
      handle: 'span.drag-handle',
    })
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 1em;
  margin: 1em 0;
  box-shadow: 0 0 1px rgba(0, 0, 0, .3);
  position: relative;
}

li span {
  position: absolute;
  content: '';
  right: 0;
  top: 0;
  bottom: 0;
  width: 3em;
  font-size: 1em;
  font-weight: bolder;
  line-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: grab;
}
</style>