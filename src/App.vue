<script setup>
import { computed, ref } from 'vue';

const hasEditing = ref(false);
const newTask = ref("");
const todos = ref([
  {task: 'study javascript', status: true},
  {task: 'eat pizza', status: false},
])

const taskDoneStyle = computed(() => {
  return todos.value.map(item => (item.status ? 'task-done' : 'task-pending'));
});


function addTask() {
  if (Boolean(newTask.value))
    todos.value.push({ task: newTask.value.toLowerCase(), status: false})
    newTask.value = ""
}

function deleteTask(item)
{
  todos.value =  todos.value.filter((t) => t !== item)
}

function editTask(item)
{
  hasEditing.value = !hasEditing.value;

  const word = newTask.value;

  if(!!word)
    todos.value.splice(item.index, 1, { task: word.toLowerCase(), status: item.status})
    newTask.value = ""
}
</script>

<template>
  <section class="main">
    <h1>Welcome to your todoZ</h1>

    <section class="add-task">
      <input v-model="newTask" :placeholder="hasEditing ? 'Edite sua tarefa' : 'Adicione uma tarefa'" type="text">
      <button v-show="!hasEditing" @click="addTask">Add task</button>
    </section>

    <ul class="main-list">
      <li v-for="(item, index) in todos" :class="taskDoneStyle[index]">
        <input type="checkbox" v-model="item.status">
        {{  item.task }}
        <button @click="editTask(item)" class="actions-button">&#128393;</button>
        <button @click="deleteTask(item)" class="actions-button">&#10007;</button>
      </li>
    </ul>
  </section>
</template>

<style scoped>
.main
{
  display: flex;
  align-items: center;
  flex-direction: column;
}

.main-list
{
  border: solid 1px rgb(75, 121, 219);
  list-style: none;
  width: 500px;
  border-radius: 5px;
  padding: 2rem;
}

.main-list li
{
  font-size: 1.2rem;
  margin: 1rem 0;
}
.task-done
{
  text-decoration-line: line-through;
}
.task-pending
{
  color: rgb(75, 121, 219);
}

.actions-button
{
  background-color: rgb(75, 121, 219);
  color: white;
  padding: 0.2rem 0.4rem;
  cursor: pointer;
  border: none;
  margin: 0 0.5rem;
  }

  .add-task input
  {
    font-size: 1.2rem;
    margin: 2rem 0.5rem;
    padding: 0.5rem;
    border: none;
    outline: none;
  }

  .add-task button
  {
    font-size: 1.2rem;
    background-color: rgb(75, 121, 219);
    margin: 2rem 0.5rem;
    padding: 0.5rem;
    border: none;
    outline: none;
  }
</style>
