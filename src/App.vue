<template>
  <div>
    <h1>Nom de la tâche {{ taskName }}</h1>
    <p>Tâche en cours</p>
    <div>
      <span v-if="isTaskInProgress">oui</span>
      <span v-if="!isTaskInProgress">non</span>
    </div>
    <input type="text" v-model="taskName">
    <button @click="startTask">Start</button>
    <button @click="stopTask">Stop</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      tasks: [],
      taskID: 0,
      taskName: '',
      isTaskInProgress: false,
      startTime: null,
      errorMsg: null,
    }
  },
  methods: {
    startTask() {
      if (this.taskName.length === 0) {
        this.errorMsg = "Le nom d'une tâche ne peut pas être vide"
        return
      } else if (this.isTaskInProgress) {
        this.errorMsg = "Une tâche est déjà en cours"
        return
      } else {
        this.errorMsg = null
      }
      this.isTaskInProgress = true
      this.startTime = Date.now()
    },
    stopTask() {
      if (!this.isTaskInProgress) {
        this.errorMsg = "Aucune tâche est en cours"
        return
      }
      this.tasks.push({
        id: this.getId(),
        name: this.taskName,
        start: this.startTime,
        end: Date.now()
      })
      this.isTaskInProgress = false
      this.errorMsg = null
      this.taskName = ''
    },
    getId() {
      this.taskID++
      return this.taskID
    },
    formatTimeStamp(ts) {
      let formatter = Intl.DateTimeFormat('fr', { hour: '2-digit', minute: '2-digit' })
      return formatter.format(ts)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
