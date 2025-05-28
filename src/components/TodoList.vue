<script setup lang="ts">
import { computed, ref } from 'vue'

interface Task {
  name: string
  status: boolean //完ならtrue, 未完ならfalse
}

const tasks = ref<Task[]>([
  { name: '物理レポート', status: false },
  { name: '化学レポート', status: false },
  { name: '数学レポート', status: true },
  { name: '英語レポート', status: false },
  { name: '線形代数レポート', status: true }
])

const finishedTasks = computed(() => {
  return tasks.value.filter((task) => task.status)
})
const notFinishedTasks = computed(() => {
  return tasks.value.filter((task) => !task.status)
})

let newTaskName = ref('')
const newTaskStatus = ref(false)

//タスクを完了する関数
const markAsFinished = (task: Task) => {
  task.status = true
}

const addTask = () => {
  if (newTaskName.value == '') return
  tasks.value.push({ name: newTaskName.value, status: newTaskStatus.value })
  newTaskName.value = ''
}
</script>

<template>
  <h2>TodoList</h2>
  <div>
    <div>
      <h3>未完のタスク</h3>
      <ul>
        <li v-for="task in notFinishedTasks" :key="task.name">
          <span  @click="markAsFinished(task)" style="cursor: pointer;">
            ⬜
          </span>
          <span>{{ task.name }}</span>
        </li>
      </ul>
    </div>
    <div>
      <h3>完了済タスク</h3>
      <ul>
        <li v-for="task in finishedTasks" :key="task.name">
          <span>
            ✅️{{ task.name }}
          </span>
        </li>
      </ul>
    </div>
  </div>
  <div>
    <label>
      タスクを追加
      <input v-model="newTaskName" type="text" @keyup.enter="addTask" />
    </label>
    <button @click="addTask">追加</button>
  </div>
</template>

<style>
li {
  list-style: none;
  margin: 0.5em 0;
}
ul {
  padding-left: 1em;
}
</style>
