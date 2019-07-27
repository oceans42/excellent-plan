<template>
  <div id="app">
    <el-container>
      <el-container>
        <el-aside width="250px"
                  class="aside">
          <div>
            <p>1.在清醒时写下三个锦囊<br>（只能写3个哦）</p>
            <p>2.在迷惘时打开看看<br>（用同一个浏览器）</p>
          </div>

          <p class="footer">备注：这是本人做的第一个小作品，正在摸索中，求轻拍~主要是给自己用。有吐槽？欢迎发这个邮箱：sparks_42@163.com</p>
        </el-aside>
        <el-main>
          <h1> <a href="#"> 锦囊妙计 </a> </h1>
          <img src="./assets/bag1.png"
               class="bag"
               @click="open(1)">
          <img src="./assets/bag2.png"
               class="bag"
               @click="open(2)">
          <img src="./assets/bag3.png"
               class="bag"
               @click="open(3)">
        </el-main>
      </el-container>
    </el-container>
    <!-- <ToDo msg="Welcome to Your Vue.js App" /> -->
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

import ToDo from "./components/ToDo.vue";

export default {
  name: "app",
  data: function() {
    return {
      todos: todoStorage.fetch()
    };
  },
  components: {
    ToDo
  },
  watch: {
    todos: {
      handler: function(todos) {
        todoStorage.save(todos);
      },
      deep: true
    }
  },
  methods: {
    open(num) {
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].id === num) {
          this.$alert(this.todos[i].title, `第${this.todos[i].id}个锦囊`, {
            confirmButtonText: "确定",
            closeOnClickModal:'true'
          });
          return;
        }
      }
      this.$prompt("输入妙计", {
        confirmButtonText: "确定",
        cancelButtonText: "取消"
      }).then(({ value }) => {
        this.addTodo(value, num);
        this.$message({
          type: "success",
          message: "你的妙计已存入"
        });
      });
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

.footer{
  color: #666
}

</style>
