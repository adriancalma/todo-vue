<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>
    <div class="input-group d-flex mt-5">
      <input
        type="text"
        class="form-control"
        placeholder="Enter Task"
        v-model="task"
      />
      <div class="input-group-append">
        <button class="btn btn-warning rounded-0" @click="submitTask">
          SUBMIT
        </button>
      </div>
    </div>
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.id">
          <td>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </td>
          <td>
            <span
              class="pointer"
              @click="changeStatus(task.id)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished',
              }"
              >{{ task.status }}</span
            >
          </td>
          <td>
            <div class="text-center pointer" @click="editTask(task.id)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center pointer" @click="deleteTask(task.id)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "TodoComponent",
  data() {
    return {
      task: null,
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [
        {
          id: 1,
          name: "Steal bananas from the store",
          status: "to-do",
        },
        {
          id: 2,
          name: "Eat 1kg chocolate in 1 hour",
          status: "in-progress",
        },
      ],
    };
  },
  methods: {
    getTaskIndex(id) {
      return this.tasks.findIndex((task) => task.id === id);
    },

    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          id: Date.now(),
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = null;
    },

    editTask(id) {
      let index = this.getTaskIndex(id);
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    deleteTask(id) {
      let index = this.getTaskIndex(id);
      this.tasks.splice(index, 1);
      this.editedTask = null;
    },

    changeStatus(id) {
      let index = this.getTaskIndex(id);

      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      ++newIndex;

      if (newIndex > 2) newIndex = 0;

      this.tasks[index].status = this.availableStatuses[newIndex];
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
tbody tr td:nth-child(2) {
  text-transform: capitalize;
  width: 120px;
}
.finished {
  text-decoration: line-through;
}
</style>
