<template>
    <div class="list-wrapper">
        <h2>{{ title }}</h2>
        <input v-model="newTask" placeholder="task">
        <button class="add" @click="add">add</button>
        <div class="drag">
        <draggable :list="list" class="dragArea">
          <div v-for="(item, index) in list" v-bind:key="item.id">
            {{ item.task }}
            <button class="remove" @click="remove(index)">remove</button>
          </div>
        </draggable>
        </div>
    </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable
  },
  name: "TodoList",
  props: {
    title: String
  },
  data() {
    return {
      newTask: "",
      list: [
        { id: 1, task: "hoge" },
        { id: 2, task: "fuga" },
        { id: 3, task: "foo" },
        { id: 4, task: "piyo" }
      ]
    };
  },
  methods: {
    add: function() {
      if (this.newTask === "") return alert("値を入力してください");

      let newList = [
        ...this.list,
        { id: this.list.length + 1, task: this.newTask }
      ];
      this.list = newList;
    },
    remove: function(index) {
      let newList = [...this.list];
      newList.splice(index, 1);

      this.list = newList;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
h3
  margin: 40px 0 0;
ul
  list-style-type: none
  padding: 0
li
  margin: 0 10px
a
  color: #42b983
</style>