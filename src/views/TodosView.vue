<template>
  <div class="todos">
    <h1>This is an todos page</h1>
    <div>
      <p>タイトル</p>
      <input type="text" v-model="title" />
    </div>
    <div>
      <button @click="addTodo">作成</button>
    </div>
    <div v-for="todo in todoList" :key="todo.id">
      <p>{{ todo.title }}</p>
      <button @click="deleteTodo(todo)">削除</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { TodoModel } from "../models";

@Component
export default class TodosView extends Vue {
  todoList = [];
  title = "";
  created() {
    this.getTodos();
  }
  async getTodos() {
    try {
      let response = await this.axios.get("http://localhost:3000/v1/todos");
      this.todoList = response.data;
    } catch (e) {
      console.log(e);
    }
  }
  async addTodo() {
    try {
      let params = {
        title: this.title,
      };
      await this.axios.post("http://localhost:3000/v1/todos", params);
      this.getTodos();
      this.title = "";
    } catch (e) {
      console.log(e);
    }
  }
  async deleteTodo(todo: TodoModel) {
    try {
      await this.axios.delete("http://localhost:3000/v1/todos/" + todo.id);
      this.getTodos();
    } catch (e) {
      console.log(e);
    }
  }
}
</script>
