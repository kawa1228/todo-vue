<template>
    <div class="list-wrapper">
        <h1>{{ title }}</h1>

        <img src="../image/img01.jpg" style="width: 80%">

        <el-form
          :inline="true"
          class="demo-form-inline"
        >
          <el-form-item label="task by">
            <el-input
              v-model="newTask"
              @keyup.13="add"
              placeholder="here"
            >
            </el-input>
          </el-form-item>

            <el-form-item>
            <el-button
            style="display: inline"
            class="add"
            @click="add"
            size="small"
            type="danger"
            round>
              add
            </el-button>
            </el-form-item>

        </el-form>

        <div class="drag">
        <draggable :list="list" class="dragArea">
          <div class="items" v-for="(item, index) in list" v-bind:key="item.id" style>
            <div class="item-name">
            {{ item.task }}
            </div>

            <el-row class="el-row">
              <el-button
                type="text"
                size="mini"
                icon="el-icon-edit"
                circle></el-button>

              <el-button
                class="remove"
                @click="remove(index)"
                size="mini"
                type="text"
                icon="el-icon-delete"
                circle>
              </el-button>

              <div v-if="list[index].star" class="star-btn"> 
                <el-button
                @click="toColor(index)"
                type="text"
                size="mini"
                icon="el-icon-star-on"
                circle></el-button>
              </div>
              <div v-else class="star-btn">
                <el-button
                @click="toColor(index)"
                type="text"
                size="mini"
                icon="el-icon-star-off"
                circle></el-button>
              </div>
            </el-row>

          </div>
        </draggable>
        </div>
      <el-button plain class="top" @click="scrollTop" icon="el-icon-arrow-up">Top</el-button>
      
    </div>
</template>

<script>
import draggable from "vuedraggable";
import smoothScroll from "smooth-scroll";

export default {
  components: {
    draggable
  },
  name: "TodoList",
  props: {
    title: String,
    plain: true
  },
  data() {
    return {
      list: [
        {
          id: 1,
          task: "HOGE",
          star: false
        },
        {
          id: 2,
          task: "fuga",
          star: false
        },
        {
          id: 3,
          task: "foo",
          star: false
        }
      ],
      newTask: ""
    };
  },
  methods: {
    add: function() {
      if (this.newTask === "") return alert("値を入力してください");
      let newList = [
        ...this.list,
        { id: this.list.length + 1, task: this.newTask, star: false }
      ];
      this.list = newList;
    },
    remove: function(index) {
      const doremove = window.confirm(`${this.list[index].task}を削除します`);
      if (!doremove) return;

      let newList = [...this.list];
      newList.splice(index, 1);
      this.list = newList;
    },
    scrollTop: function() {
      const scroll = new smoothScroll();
      scroll.animateScroll(0);
    },
    toColor: function(index) {
      this.list[index].star = !this.list[index].star;

      console.log(this.list[index].star);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
@import "../styles/TodoList.sass"
</style>