<template>
    <div class="list-wrapper">
        <h1>{{ title }}</h1>

        <img src="../image/img01.jpg" style="width: 80%">

        <el-form
          :inline="true"
          class="demo-form-inline"
          onsubmit="return false"
        >
          <!-- <label v-show="validation/*addtaskBlankFlag*/" class="label-warning">※値を入力してください</label><br> -->
          <label v-show="addtaskBlankFlag" class="label-warning">※値を入力してください</label><br>
          <el-form-item label="task by">
            <el-input
              v-model="newTask"
              placeholder="here"
              @keyup.enter.native="add"
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

        <el-row class="priority">
          <el-button @click="sortPriority" type="warning" size="mini" plain><i class="el-icon-sort"></i>　sort</el-button>
        </el-row>
      
        <div class="drag">
        <draggable :list="list" class="dragArea">
          <div class="items" v-for="(item, index) in list" v-bind:key="item.id" style>
            <div class="item-name"
            v-bind:class="{ checkd: list[index].check }"
            >
            {{ item.task }}
            </div>

            <el-row class="el-row">
              <el-button
                @click="editFlagChange(index)"
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
                @click="changeStar(index)"
                type="text"
                size="mini"
                icon="el-icon-star-on"
                circle></el-button>
              </div>
              <div v-else class="star-btn">
                <el-button
                @click="changeStar(index)"
                type="text"
                size="mini"
                icon="el-icon-star-off"
                circle></el-button>
              </div>

              <el-button
                @click="doCheck(index)"
                class="check-btn"
                type="text"
                size="mini"
                icon="el-icon-circle-check-outline"
                cicle></el-button>
            </el-row>

          </div>
        </draggable>
        </div>

        <el-dialog title="Edit task" :visible.sync="dialogFormVisible">
         <el-form :model="form" onsubmit="return false">
           <el-form-item>
             <label v-show="editBlankFlag" class="label-warning">※値を入力してください</label>
             <el-input
             v-model="form.name"
             placeholder="task name"
             @keyup.enter.native="editTask"
             ></el-input>
           </el-form-item>
         </el-form>
           <el-button @click="dialogFormVisible = false">Cancel</el-button>
           <el-button type="primary" @click="editTask">Confirm</el-button>
       </el-dialog>

      <!-- scroll -->
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
          edit: false,
          star: false,
          check: false
        },
        {
          id: 2,
          task: "fuga",
          edit: false,
          star: false,
          check: false
        },
        {
          id: 3,
          task: "foo",
          edit: false,
          star: false,
          check: false
        }
      ],
      newTask: "",
      dialogFormVisible: false,
      form: {
        name: ""
      },
      addtaskBlankFlag: false,
      editBlankFlag: false
    };
  },
  // computed: {
  //   validation: function() {
  //     return !this.newTask;
  //   }
  // },
  methods: {
    add: function() {
      if (this.newTask === "") {
        return (this.addtaskBlankFlag = true);
      } else {
        this.addtaskBlankFlag = false;
      }

      let maxid = 0;
      this.list.forEach(i => {
        if (maxid < i.id) maxid = i.id;
      });

      let newList = [
        ...this.list,
        {
          id: maxid + 1,
          task: this.newTask,
          edit: false,
          star: false,
          check: false
        }
      ];

      let doAdd = true;
      this.list.forEach(i => {
        if (i.task === this.newTask) {
          doAdd = window.confirm("同じタスクです。追加しますか？");
        }
      });
      if (!doAdd) return;
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
    changeStar: function(index) {
      this.list[index].star = !this.list[index].star;
    },
    editFlagChange: function(index) {
      this.dialogFormVisible = true;

      this.list[index].edit = true;
    },
    editTask: function() {
      if (this.form.name === "") return (this.editBlankFlag = true);

      this.list.forEach(v => {
        if (v.edit) {
          v.task = this.form.name;
          v.edit = false;
        }
      });
      this.dialogFormVisible = false;
    },
    sortPriority: function() {
      let onList = [];
      let offList = [];
      this.list.forEach(i => {
        i.star ? onList.push(i) : offList.push(i);
      });
      let newList = onList.concat(offList);
      this.list = newList;
    },
    doCheck: function(index) {
      this.list[index].check = !this.list[index].check;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
@import "../styles/TodoList.sass"
</style>