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
                plain=plain
                size="mini"
                icon="el-icon-edit"
                circle></el-button>

              <el-button
                class="remove"
                @click="remove(index)"
                size="mini"
                type="text"
                plain=plain
                icon="el-icon-delete"
                circle>
              </el-button>
               
              <el-button
                type="text"
                plain=plain
                size="mini"
                icon="el-icon-star-off"
                circle></el-button>
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
          task: "HOGE"
        },
        {
          id: 2,
          task: "fuga"
        },
        {
          id: 3,
          task: "foo"
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
        { id: this.list.length + 1, task: this.newTask }
      ];
      this.list = newList;
    },
    remove: function(index) {
      let newList = [...this.list];
      newList.splice(index, 1);
      this.list = newList;
    },
    scrollTop: function() {
      const scroll = new smoothScroll();
      scroll.animateScroll(0);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
h1
  letter-spacing: 7px;
img
  padding-bottom: 20px
h3
  margin: 40px 0 0;
ul
  list-style-type: none
  padding: 0
li
  margin: 0 10px
a
  color: #42b983
.el-row
  display: inline-block
.top
  margin-top: 20px
.items
  width: 80%
  margin: 5px
  padding: 4px
  border: solid 1px #dcdfe6
  border-radius: 4px
  display: inline-block
  cursor: pointer
.items:hover
  border: solid 1px #409EFF
  transition: border 0.5s
.item-name
  display: inline-block
  padding: 10px 40px
  font-weight: 500;
  letter-spacing: 2px;
</style>