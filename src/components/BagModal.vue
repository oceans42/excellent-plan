<template>
  <el-dialog class="dialog"
             :title='"第"+bag.id+"个锦囊"'
             :visible.sync="visible"
             width="30%"
             :before-close="handleClose">
    <span>{{bag.title}}</span>
    <span slot="footer"
          class="dialog-footer">
      <el-button type="danger"
                 size="mini"
                 @click="removeTodo(bag)"
                 class="delete"
                 icon="el-icon-delete"
                 circle></el-button>
      <el-button type="primary"
                 size="mini"
                 @click="handleClose()">知道了</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    handleClose() {
      this.$emit("hide");
    },
    removeTodo(bag) {
      this.$emit("hide");
      this.$emit("removeBag", bag);
    }
  },
  props: ["visible", "bag"]
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
</style>
