<template>
  <div class="container">
    <h2 class="text-center mt-5">To Do List</h2>

    <!-- Input -->
    <div class="d-flex">
      <input
        type="text"
        placeholder="insert new task"
        class="form-control form-control-lg mt-2"
        name="todo-list"
        style="margin-right: 10px"
        v-model="task"
      />
      <button class="btn btn-success rounded-2 mt-2" @click="submitTask">
        Submit
      </button>
    </div>

    <div class="table mt-4">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Task</th>
            <th scope="col">Status</th>
            <th scope="col" colspan="2" style="text-align: center">Action</th>
            <!-- <th scope="col">#</th> -->
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td
              style="width: 350px"
              :class="{ completed: task.status === 'Completed' }"
            >
              {{ task.todo }}
            </td>

            <td
              class="pointer"
              @click="changeStatus(index)"
              style="width: 150px"
              :class="{
                'text-primary': task.status === 'In Progress',
                'text-success': task.status === 'Completed',
                'text-danger': task.status === 'Cancel',
              }"
            >
              {{ task.status }}
            </td>

            <td>
              <div
                class="align-items-center pointer text-center action"
                @click="editTask(index)"
              >
                <span class="fa fa-pen"></span>
              </div>
            </td>

            <td>
              <div
                class="align-items-center pointer text-center action"
                @click="deleteTask(index)"
              >
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
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatus: ["In Progress", "Completed", "Cancel"],
      tasks: [
        {
          todo: 'Watch "Attack on Titan Final Chapter"',
          status: "In Progress",
        },
        {
          todo: 'Read book: "Atomic Habits"',
          status: "Cancel",
        },
        {
          todo: "Make portofolio with React.js",
          status: "In Progress",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          todo: this.task,
          status: "In Progress",
        });
      } else {
        this.tasks[this.editedTask].todo = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      console.log(this.tasks);
    },
    editTask(index) {
      this.task = this.tasks[index].todo;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
      // console.log(newIndex, "Ini new indexnya");

      if (++newIndex > 2) {
        newIndex = 0;
      }

      this.tasks[index].status = this.availableStatus[newIndex];
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
/* .action {
  width: 20px;
  text-align: center;
} */
</style>
