<template>
  <!-- Main Container for Todo Task APP -->
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>
    <!-- input div -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        SUBMIT
      </button>
    </div>
    <!-- Task table -->
    <div>
      <table class="table table-bordered mt-5">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col" class="text-center">Edit</th>
            <th scope="col" class="text-center">Delete</th>
          </tr>
        </thead>
        <tbody>
          <!-- displaying array list from the data function -->
          <!-- using v-for directive from vue -->
          <tr v-for="(task, index) in tasks" :key="index">
            <th>
              <span :class="{ finished: task.status === 'done' }">{{
                task.name
              }}</span>
            </th>
            <td style="width: 120px">
              <span
                @click="changeStatus(index)"
                class="pointer"
                :class="{
                  'text-danger': task.status === 'to-do',
                  'text-warning': task.status === 'in-progress',
                  'text-success': task.status === 'done',
                }"
                >{{ firstCharUpper(task.status) }}</span
              >
            </td>
            <td>
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen"></span>
              </div>
            </td>
            <td>
              <div class="text-center" @click="deleteTask(index)">
                <span class="fa fa-trash"></span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      editedTask: null,
      task: '',
      availableStatuses: ['to-do', 'in-progress', 'done'],
      tasks: [
        {
          name: 'Steal bananas',
          status: 'to-do',
        },
        {
          name: 'Make bananas milkshake',
          status: 'in-progress',
        },
        {
          name: 'Drink bananas milkshake',
          status: 'to-do',
        },
      ],
    }
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return
      if (this.editedTask == null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do',
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }

      this.task = ''
    },
    deleteTask(index) {
      // deleting items from task array using splice function
      this.tasks.splice(index, 1)
    },
    // editing tasks
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },

    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)

      if (++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.availableStatuses[newIndex]
    },

    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    },
  },
}
</script>
<style>
.pointer {
  cursor: pointer;
}

.finished {
  text-decoration: line-through;
}
</style>
