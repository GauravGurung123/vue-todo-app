<template>
  <h2 class="text-center mt-5">Todoapp</h2>
  <q-banner class="bg-primary text-white" v-show="success">
    Task Added Successfully.
  </q-banner>
  <form @submit.prevent="submitTask">
    <div class="q-pa-md">
      <div class="q-gutter-y-md column" style="max-width: 300px">
        <q-input
          v-model="fields.title"
          name="title"
          clearable
          color="purple-12"
          label="Add new task "
        />
        <q-btn label="Submit" type="submit" color="secondary" />
      </div>
    </div>
  </form>
  <div class="q-pa-md">
    <q-card class="my-card">
      <q-card-section>
        <div class="text-h6">To Do List</div>
      </q-card-section>

      <q-markup-table>
        <thead>
          <tr>
            <th>S.N</th>
            <th>Task</th>
            <th class="text-left">Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(task, index) in tasks" :key="index">
            <td>{{ ++index }}</td>
            <td>
              {{ task.title }}
              <q-popup-edit
                v-model="task.title"
                title="Edit the task"
                auto-save
              >
                <form @submit.prevent="updateTask(task.id)">
                  <q-input
                    v-model="fields.title"
                    name="title"
                    dense
                    autofocus
                    counter
                  />
                  <q-btn label="Update" type="submit" color="primary" />
                </form>
              </q-popup-edit>
            </td>
            <td>
              {{ task.status }}
              <q-popup-edit
                v-model="task.status"
                title="Edit the status"
                auto-save
                v-slot="scope"
              >
                <q-input
                  v-model="scope.value"
                  name="title"
                  dense
                  autofocus
                  counter
                  @keyup.enter="scope.set"
                />
              </q-popup-edit>
            </td>

            <td>
              <form @submit.prevent="deleteTask(index, task.id)">
                <q-btn label="Del" type="submit" color="primary" />
              </form>
            </td>
          </tr>
        </tbody>
      </q-markup-table>
    </q-card>
  </div>
</template>

<script>
export default {
  name: "ToDoApp",

  data() {
    return {
      fields: {},
      tasks: null,
      success: false,
    };
  },

  mounted() {
    this.refresh();
  },
  methods: {

    refresh(){
      axios.get("http://127.0.0.1:8000/api/tasks").then((res) => {
      this.tasks = res.data.data;
      console.log(this.tasks);
    });
    },
    submitTask() {
      axios
        .post("http://127.0.0.1:8000/api/tasks", this.fields)
        .then((res) => {
          this.fields = {};
          // location.reload();
          this.refresh()
          this.success = true;
        })
        .catch((error) => {
          console.log("Error");
        });
    },

    updateTask(id) {
      axios
        .put("http://127.0.0.1:8000/api/tasks/" + id, this.fields)
        .then((res) => this.refresh() )//location.reload())
        .catch((error) => console.log(error));
    },

    deleteTask(index, id) {
      axios
        .delete("http://127.0.0.1:8000/api/tasks/" + id)
        .then((res) => {
          this.refresh()
          // location.reload();
          // this.tasks.splice(index, 1);
        })
        .catch((error) => {
          console.log("error while deleting");
        });
    },
  },
};
</script>
