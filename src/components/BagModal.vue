<template>
  <el-dialog class="dialog"
             :title='"第"+bag.id+"个锦囊"'
             :visible.sync="visible"
             :close-on-click-modal="false"
             width="30%"
             :before-close="handleClose"
             :class="{ editing: bag == editedTodo }">
    <span v-if="bag != editedTodo">{{bag.title}}</span>
    <input class="edit"
           type="text"
           v-model="bag.title"
           v-todo-focus="bag == editedTodo"
           @blur="doneEdit(bag)"
           @keyup.enter="doneEdit(bag)"
           @keyup.esc="cancelEdit(bag)">
    <span slot="footer"
          class="dialog-footer">
      <el-button type="danger"
                 size="mini"
                 @click="remove(bag)"
                 class="delete"
                 icon="el-icon-delete"
                 circle></el-button>
      <el-button type="primary"
                 size="mini"
                 class="editButton"
                 @click="edit(bag)"
                 icon="el-icon-edit"
                 circle></el-button>
      <el-button type="success"
                 size="mini"
                 @click="handleClose()">关闭</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      editedTodo: null
    };
  },
  methods: {
    handleClose() {
      this.$emit("hide");
    },
    remove(bag) {
      this.$emit("hide");
      this.$emit("removeBag", bag);
    },
    edit: function(bag) {
      this.beforeEditCache = bag.title;
      this.editedTodo = bag;
    },
    doneEdit: function(bag) {
      if (!this.editedTodo) {
        return;
      }
      this.editedTodo = null;
      bag.title = bag.title.trim();
      if (!bag.title) {
        this.remove(bag);
      }
    }
  },
  props: ["visible", "bag"],
  directives: {
    "todo-focus": function(el, binding) {
      if (binding.value) {
        el.focus();
      }
    }
  }
};
</script>

<style scoped>
.dialog{
  text-align: left;
}
.delete{
    position: absolute;
    left: 16px;
}
.editButton{
    position: absolute;
    left: 43px;
}
.dialog.editing .edit {
  display: block;
}
.edit {
  display: none;
    position: relative;
    margin: 0;
    width: 100%;
    font-family: inherit;
    font-weight: inherit;
    line-height: 1.4em;
    border: 0;
    color: inherit;
    padding: 6px;
    border: 1px solid #999;
    box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
    box-sizing: border-box;
}

</style>
