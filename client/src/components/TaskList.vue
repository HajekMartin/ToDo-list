<template>
  <div class="list-wrapper">
    <ul class="d-flex flex-column-reverse todo-list">
      <li
        v-for="item in tasks"
        :key="item.id"
        v-bind:class="{ completed: item.completed }"
      >
        <div class="form-check">
          <label class="form-check-label">
            <input
              v-on:click="TaskComplete(item.id, item.content)"
              class="checkbox"
              type="checkbox"
              :checked="item.completed ? true : null"
            />
            <i class="input-helper"></i>{{ item.content }}</label
          >
        </div>
        <div class="task-icons">
          <b-icon
            icon="trash"
            class="remove mdi"
            v-on:click="TaskDelete(item.id)"
          ></b-icon>
          <b-icon
            icon="pencil"
            class="edit mdi"
            v-b-modal.modal-edit
            v-on:click="TaskEdit(item.id)"
          ></b-icon>
        </div>
      </li>
    </ul>
    <ModalEdit></ModalEdit>
  </div>
</template>

<script>
import axios from "axios";
import ModalEdit from "./ModalEdit.vue";
export default {
  name: "TaskList",
  data() {
    return {
      tasks: [],
      timer: undefined,
      filter: null,
      connection: null,
    };
  },
  methods: {
    Refresh() {
      var url_filt = this.$route.query.filter;
      if (url_filt !== undefined) {
        if (url_filt == null) {
          this.filter = null;
        } else {
          this.filter = url_filt;
        }
      }
      if (this.filter == null) {
        //console.log("TaskList.Refresh => NULL");
        axios.get(process.env.VUE_APP_URL + "/api/tasks/").then((response) => {
          this.tasks = response.data;
        });
      } else {
        //console.log("TaskList.Refresh => ACTIVE");
        axios
          .get(process.env.VUE_APP_URL + "/api/tasks/completed/" + this.filter)
          .then((response) => {
            this.tasks = response.data;
          });
      }
    },
    TaskDelete: async function (id) {
      console.log("TaskList.TaskDelete");
      await axios.delete(process.env.VUE_APP_URL + "/api/tasks/" + id);
      this.connection.send("");
    },
    TaskComplete: async function (id, content) {
      console.log("TaskList.TaskComplete");
      var result = await axios.get(
        process.env.VUE_APP_URL + "/api/tasks/" + id
      );
      var c = 1;
      if (result.data.completed === 1) {
        c = 0;
      }
      await axios.put(process.env.VUE_APP_URL + "/api/tasks/" + id, {
        content: content,
        completed: c,
      });
      this.connection.send("");
    },
    TaskEdit(task_id) {
      console.log("TaskList.TaskEdit" + task_id);
      this.$router
        .push({ path: "todolist", query: { id: task_id } })
        .catch(() => {});
    },
  },
  created() {
    this.tasks = [];
    let self = this;
    this.connection = new WebSocket(process.env.VUE_APP_SOCKET_URL);
    this.connection.onopen = function (event) {
      console.log("TaskList connected");
      console.log(event);
    };
    this.connection.onmessage = function (event) {
      if (event.data === "refresh") {
        self.Refresh();
      }
    };
    self.Refresh();
  },
  components: {
    ModalEdit,
  },
};
</script>

<style lang="css" scoped>
@import "../assets/ToDoListStyle.css";
</style>
