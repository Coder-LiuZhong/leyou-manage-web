<template>
  <v-card>    <!--vuetitfy里面的组件，包含成一个悬浮效果的card-->
      <v-flex xs12 sm10>    <!--vuetitfy里面的布局里的-->
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>
  export default {
    name: "category",
    data() {
      return {
        isEdit:true
      }
    },
    methods: {
      handleAdd(node) {
        console.log("add .... ");
        console.log(node);
      },
      handleEdit(id, name) {
        console.log("edit... id: " + id + ", name: " + name)
      },
      handleDelete(id) {
        console.log("delete ... " + id)
      },
      handleClick(node) {
        console.log(node)
      }
    }
  };
</script>

<style scoped>

</style>
