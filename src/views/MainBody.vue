<script setup>
import { ref, computed } from 'vue'

const taskList = ref([]);
const newTask = ref('')

const pendingTasks = computed(() => {
  return taskList.value.filter((list) => list.isCompleted === false).length;
})

function addTask() {
  if (newTask.value) {
    taskList.value.push({ name: newTask.value, isCompleted: false })
  }
  newTask.value = ''
}

function clearAll() {
  taskList.value = []
}

function removeTask(index) {
  taskList.value.splice(index, 1)
}

function completeTask(task) {
  task.isCompleted = !task.isCompleted;
}
function clearCompleted() {
  taskList.value = taskList.value.filter((list) => list.isCompleted === false)
}

</script>

<template id="task-list">
  <h1 id="tagLine">Just Do It.</h1>
  <h1>Tasks <span v-if="pendingTasks">({{ pendingTasks }})</span></h1>
  <section class="tasks">
    <div class="d-flex justify-content-evenly m-2">
      <input class="input-field-task" type="text" v-model="newTask" placeholder="Add a task" @keyup.enter="addTask">
      <button type="button" class="btn btn-outline-light" @click="addTask">Add </button>
    </div>

    <div class="d-flex justify-content-end ">
      <button type="button" class="btn btn-outline-warning me-2" @click="clearCompleted"> Clear Completed </button>
      <button type="button" class="btn btn-outline-danger me-2" @click="clearAll"> Clear All </button>
    </div>

    <TransitionGroup name="list" tag="ul" class="list-group mt-2">
      <li class="list-group-item d-flex m-2" v-for="(task, index) in taskList" :task="task" :key="task">
        <button type="button" :class="['btn', 'me-2', 'taskListItem', { 'completed': task.isCompleted }]"
          @click="completeTask(task)">
          {{ task.name }}
        </button>
        <button type="button" class="btn btn-outline-danger" @click="removeTask(index)"> Remove </button>
      </li>
    </TransitionGroup>

  </section>
</template>

<style scoped>
#tagLine {
  font-size: 100px;
  text-align: center;
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif
}

.taskListItem {
  width: 100%;
  text-align: left;
}

.taskListItem.completed {
  background-color: #faffc4;
  text-decoration-line: line-through;
}

.input-field-task {
  width: 100%;
  border-radius: 2px;
  margin-right: 5px;
  font-size: 20px;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  transform: translateX(50px);
  opacity: 0;
}
</style>