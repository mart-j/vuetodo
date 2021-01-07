<template>
  <div class="row">
    <div class="col-xs-offset-3 col-xs-6 col-md-offset-4 col-md-4">
      <form @submit.prevent="addTodo">
        <h1 class="heading">Todo List</h1>
        <div class="inputWrapper">
          <input class="input" placeholder="Buy milk" v-model="inputValue" />
          <div class="buttonWrapper">
            <Button :type="'submit'" :label="add" />
          </div>
        </div>
        <div class="undone">
          <button @click="showUndone = !showUndone" type="button">
            {{
              showUndone && undoneTodos().length > 0
                ? 'Hide undone'
                : 'Show undone'
            }}
          </button>
          <div
            v-if="showUndone && undoneTodos().length > 0"
            class="undoneTasks"
          >
            <div class="undoneTask" v-for="(todo, i) in undoneTodos()" :key="i">
              {{ todo.title }}
            </div>
          </div>
        </div>
        <div v-if="todos.length" class="todosWrapper">
          <div
            class="todoItemWraper"
            v-for="({ title, compleated, editable }, i) in todos"
            :key="i"
          >
            <div>
              <input
                v-if="!editable"
                class="checkbox"
                @change="compleateTodo(i)"
                type="checkbox"
              />
              <span
                class="todoItem"
                v-if="!editable"
                :class="{ done: compleated }"
                >{{ title }}</span
              >
              <span class="editInputWrapper" v-else>
                <input
                  class="editInput"
                  type="text"
                  v-model="todos[i].editableValue"
                />
                <Button
                  @clickHandler="
                    (todos[i].editable = false),
                      (todos[i].editableValue = todos[i].title)
                  "
                  :label="remove"
                  class="cancelWrapper"
                />
              </span>
            </div>
            <div class="options">
              <div
                @click="editTodoHandler(i), saveEditedTodo(i, editable)"
                class="edit"
              >
                <Button v-if="!editable" :label="edit2" />
                <Button v-else :label="save" />
              </div>
              <div v-if="!editable" class="remove">
                <Button :label="remove" @clickHandler="removeTodo(i)" />
              </div>
            </div>
          </div>
        </div>
        <div v-else>no todos</div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import edit2 from './assets/edit.svg';
import edit from './assets/edit.png';
import add from './assets/add.png';
import remove from './assets/remove.png';
import save from './assets/save.png';
import Button from './components/button/Button.vue';

type Todo = {
  title: string;
  compleated: boolean;
  editable: boolean;
  editableValue: string;
};

interface Data {
  inputValue: string;
  todos: Todo[];
  edit: string;
  edit2: string;
  remove: string;
  save: string;
  add: string;
  showUndone: boolean;
}

export default defineComponent({
  name: 'App',
  components: {
    Button,
  },
  data(): Data {
    return {
      inputValue: '',
      todos: [],
      edit,
      edit2,
      remove,
      save,
      add,
      showUndone: false,
    };
  },

  methods: {
    addTodo() {
      if (this.inputValue) {
        this.todos.push({
          title: this.inputValue,
          editableValue: this.inputValue,
          compleated: false,
          editable: false,
        });
        this.inputValue = '';
      }
    },
    removeTodo(index: number) {
      const newTodos = [...this.todos];
      newTodos.splice(index, 1);
      this.todos = newTodos;
    },
    compleateTodo(index: number) {
      const newTodos = [...this.todos];
      newTodos[index].compleated = !newTodos[index].compleated;
      this.todos = newTodos;
    },
    editTodoHandler(index: number) {
      const newTodos = [...this.todos];
      newTodos[index].editable = !newTodos[index].editable;
      this.todos = newTodos;
    },
    saveEditedTodo(index: number, edita: boolean) {
      const newTodos = [...this.todos];
      if (edita && newTodos[index].editableValue) {
        newTodos[index].title = newTodos[index].editableValue;
        this.todos = newTodos;
      }
    },
    undoneTodos(): Todo[] {
      return this.todos.filter((todo) => !todo.compleated);
    },
  },
});
</script>

<style lang="scss" scoped>
@import './globalStyles/flexboxgrid.min.scss';
@import './globalStyles/index.scss';

.heading {
  text-align: center;
}

.inputWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.undone {
  position: relative;
}

.undoneTasks {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-direction: column;
  min-width: 400px;
  font-size: 20px;
  background-color: rgba(0, 0, 0, 0.842);
  color: white;
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
  z-index: 2;
  padding: 20px;
}

.undoneTask {
  margin: 5px;
  border-bottom: 1px solid white;
}

.checkbox {
  margin-right: 8px;
}

.input {
  outline: none;
  padding: 3px;
  font-size: 18px;
  width: 100%;
  border: none;
  border-bottom: 1px solid black;
  &:focus {
    transform: scale(1.01);
  }
}
.editInputWrapper {
  position: relative;
}

.todoItem {
  word-break: break-all;

  padding-right: 50px;
}

.cancelWrapper {
  cursor: pointer;
  outline: none;
  width: 22px;
  height: 22px;
  border-radius: 50%;
  border: 1px solid black;
  background-color: white;
  position: absolute;
  top: -10px;
  right: -10px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.editInput {
  outline: none;
  padding: 3px;
}
.buttonWrapper {
  margin-left: 5px;
}

.todosWrapper {
  display: flex;

  flex-direction: column;
}

.todoItemWraper {
  display: flex;
  justify-content: space-between;
  font-size: 20px;
  padding: 8px;
  margin: 2px;
  display: inline-flex;
  border-bottom: 1px solid rgba(0, 0, 0, 0.212);
}
.options {
  cursor: pointer;
  display: flex;
  position: relative;
}

.dot {
  border-radius: 50%;
  margin-right: 1px;
  width: 6px;
  height: 6px;
  background-color: rgb(66, 66, 66);
}
.done {
  text-decoration: line-through;
}

.edit,
.remove {
  display: flex;
  align-items: flex-end;
  right: 0;
  background-color: white;
  color: black;
  position: absolute;
  position: absolute;

  top: 50%;
  transform: translate(-50%, -50%);
}

.remove {
  right: 40px;
}
</style>
