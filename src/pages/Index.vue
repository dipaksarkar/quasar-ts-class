<template>
  <q-page padding>
    <q-list bordered>
      <todo-card
        @change:complete="complete(todo)"
        @change:title="(val) => todo.title = val"
        @delete="onDelete"
        v-for="todo in todos"
        :key="todo.id"
        v-bind="todo"
      ></todo-card>
    </q-list>
  </q-page>
</template>

<script lang="ts">
import { Todo } from 'components/models';
import TodoCard from 'src/components/TodoCard.vue';
import { Vue, Options } from 'vue-class-component'
import { api } from 'boot/axios'

@Options({
  components: { TodoCard }
})
export default class PageIndex extends Vue {
  todos: Todo[] = [];
  loadPost() {
    return new Promise<Todo[]>((resolve, reject) => {
      api.get('/todos').then(({ data }) => {
        resolve(data)
      }).catch((error) => {
        reject(error)
      })
    })
  }
  complete(todo: Todo) {
    todo.complete = !todo.complete
    this.$q.notify({
      message: `${todo.complete ? '[Completed]' : '[Todo]'} ${todo.title}`,
      position: 'bottom-right',
      icon: 'done'
    })
  }
  onDelete (id: number) {
    if (confirm('do you want to delete?')) {
      this.todos = this.todos.filter(item => item.id !== id)
    }
  }
  mounted() {
    this.loadPost().then(response => {
      this.todos = response
    }).catch(error => {
      console.log('error', error)
    })
  }
};
</script>
