<template>
  <Draggable
    :list="listItems"
    handle=".drag-handle"
    :class="{
      empty: !listItems.length,
      ul: true
    }"
    :group="{
      name: 'target',
      pull: false,
      put: ['source']
    }"
  >
    <div
      v-for="(item, i) in listItems"
      :key="i"
      class="item"
    >
      {{ item }}
      <span class="drag-handle">≡</span>
    </div>
  </Draggable>
</template>

<script>
import { VueDraggableNext } from 'vue-draggable-next'

export default {
  name: 'SortableList',
  components: {
    Draggable: VueDraggableNext
  },
  data() {
    return {
      listItems: [],
      dragover: false
    }
  }
}
</script>

<style scoped>
div.ul {
  list-style: none;
  padding: 0;
  margin: 0;
  min-height: 3em;
}

div.ul.dragover {
    border: 1px solid red;
  }

div.ul.empty {
  box-shadow: 0 0 10px inset rgba(0, 0, 0, .3);
  /* border: 1px solid red; */
}


div.item {
  padding: 1em;
  margin: 1em 0;
  box-shadow: 0 0 10px rgba(0, 0, 0, .3);
  position: relative;
}

div.item span {
  align-items: center;
  bottom: 0;
  content: '';
  cursor: grab;
  display: flex;
  font-size: 1em;
  font-weight: bolder;
  justify-content: center;
  line-height: 100%;
  position: absolute;
  right: 0;
  top: 0;
  width: 3em;
}
</style>