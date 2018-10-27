<template>
    <div class="list-wrapper">
        <h3>{{ msg }}</h3>
        <button class="add" @click="add">add</button>
        <button class="remove" @click="remove">remove</button>
        <ul class="list">
            <li v-for="item in list" v-bind:key="item.id">
               {{ item.task }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
  name: 'TodoList',
  props: {
    msg: String
  },
  data() {
      return {
        name: '追加',
        list: [
            { id: 1, task: '買う' },
            { id: 2, task: '売る' }
        ]
      }
  },
  methods: {
      add: function() {
          console.log('add')
      },
      remove: () => {
          console.log('remove')
      }
  }
}
</script>
