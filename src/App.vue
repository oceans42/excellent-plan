<template>
  <div id="app">
    <el-row :gutter="10">
      <el-col :xs="24"
              :sm="6"
              :md="6"
              :lg="6"
              :xl="6">
        <div class="grid-content bg-purple aside">
          <pre id="code"></pre>
        </div>
      </el-col>
      <el-col :xs="24"
              :sm="18"
              :md="18"
              :lg="18"
              :xl="18">
        <div class="grid-content bg-purple-light">
          <h1> <a href="#"> 锦囊妙计 </a> </h1>
          <img v-for="bag in bags"
               :key='bag.id'
               :src="bag.src"
               class="bag"
               @click="open(bag)">
        </div>
      </el-col>
    </el-row>
    <BagModal v-if="show"
              :visible="true"
              @hide="hide"
              @removeBag="removeBag"
              :bag="bag">
    </BagModal>
  </div>
</template>

<script>
var STORAGE_KEY = "excellent-plan";
var todoStorage = {
  fetch: function() {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    return todos;
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  }
};

import BagModal from "./components/BagModal.vue";

export default {
  name: "app",
  data: function() {
    return {
      todos: todoStorage.fetch(),
      show: false,
      bags: [
        {
          src: require("./assets/bag1.png"),
          id: 1
        },
        {
          src: require("./assets/bag2.png"),
          id: 2
        },
        {
          src: require("./assets/bag3.png"),
          id: 3
        }
      ],
      bag: null,
      leftText: `
1.在清醒时写下三个锦囊（只能写3个哦）
2.在迷惘时打开看看（用同一个浏览器）

备注：这是本人做的第一个小作品，
     正在摸索中，主要是给自己用。😄
      `
    };
  },
  components: {
    BagModal
  },
  watch: {
    todos: {
      handler: function(todos) {
        todoStorage.save(todos);
      },
      deep: true
    }
  },
  mounted() {
    this.writeCss(this.leftText);
  },
  methods: {
    open(bag) {
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].id === bag.id) {
          this.show = true;
          this.bag = this.todos[i];
          return;
        }
      }
      this.$prompt("输入妙计", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputType: "textarea"
      })
        .then(({ value }) => {
          let v = value && value.trim();
          if (!v) {
            return;
          }
          this.addTodo(v, bag.id);
          this.$message({
            type: "success",
            message: "你的妙计已存入"
          });
        })
        .catch(() => {});
    },
    addTodo: function(text, num) {
      let value = text && text.trim();
      if (!value) {
        return;
      }
      this.todos.push({
        id: num,
        title: value
      });
    },
    removeBag(bag) {
      this.todos.splice(this.todos.indexOf(bag), 1);
      this.open(bag);
    },
    hide() {
      this.show = false;
    },
    writeCss(code) {
      let domCode = document.querySelector("#code");
      let n = 0;
      let id = setInterval(() => {
        n += 1;
        domCode.innerHTML = code.substring(0, n);
        if (n >= code.length) {
          window.clearInterval(id);
        }
      }, 70);
    }
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.bag{
  width: 150px;
}
.bag:hover {
  animation: shake 0.82s cubic-bezier(.36,.07,.19,.97) both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
  cursor: pointer;
}
@keyframes shake {
  10%, 90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%, 80% {
    transform: translate3d(2px, 0, 0);
  }

  30%, 50%, 70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%, 60% {
    transform: translate3d(4px, 0, 0);
  }
}
h1 {
    position: relative;
    font-size: 70px;
    margin-top: 0;
    font-family: 'Lobster', helvetica, arial;
}

h1 a {
    text-decoration: none;
    color: #666;
    position: absolute;
    -webkit-mask-image: -webkit-gradient(linear, left top, left bottom, from(rgba(0,0,0,1)), color-stop(50%, rgba(0,0,0,.5)), to(rgba(0,0,0,1)));
}

h1:after {
    content : '锦囊妙计';
    color: #d6d6d6;
    text-shadow: 0 1px 0 white;
}

.aside{
  color: #fff;
  text-align: left;
  display: flex;
  flex-direction:column;
  justify-content: space-between;
}
pre{
  font-size: 15px;
  font-family: "微软雅黑";
  margin: 0;
  line-height: 1.5em;
}
</style>