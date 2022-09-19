<template>
  <div>
    <v-list flat subheader three-line>
      <v-card flat>
        <v-snackbar v-model="snackbar" absolute bottom right color="success">
          <span>Registration successful!</span>
          <v-icon dark> mdi-checkbox-marked-circle </v-icon>
        </v-snackbar>
        <v-snackbar v-model="deletado" absolute bottom right color="red">
          <span>Deletion successful!</span>
          <v-icon dark> mdi-checkbox-marked-circle </v-icon>
        </v-snackbar>
        <v-form ref="form" @submit.prevent="createTask">
          <v-container fluid>
            <v-row>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="form.first"
                  :rules="rules.name"
                  color="purple darken-2"
                  label="Nome da nova tarefa"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="form.last"
                  :rules="rules.name"
                  color="blue darken-2"
                  label="Descrição da tarefa"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <v-card-actions>
            <v-btn :disabled="!formIsValid" text color="primary" type="submit">
              Registrar nova tarefa
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
      <v-col cols="12"> </v-col>
      <v-subheader>Tarefas:</v-subheader>
      <v-list-item-group
        multiple
        active-class=""
        style="display: flex; flex-direction: column-reverse"
      >
        <div v-for="(tarefa, index) in tarefas" :key="index">
          <TarefaView
            :tarefa="tarefa"
            @salvarClick="recebiSalvar(index)"
            @mandarDel="deletarTarefa(tarefa)"
          />
        </div>
      </v-list-item-group>
    </v-list>
  </div>
</template>

<script>
import TasksApi from "@/api/tasks.api.js";
import TarefaView from "../components/tarefas/TarefaView.vue";
import tasksApi from "@/api/tasks.api.js";

export default {
  name: "HomeView",

  components: {
    TarefaView,
  },
  data() {
    return {
      form: {
        first: "",
        last: "",
      },
      rules: {
        name: [(val) => (val || "").length > 0 || "This field is required"],
      },
      snackbar: false,
      deletado: false,
      tarefas: [],
    };
  },
  computed: {
    formIsValid() {
      return this.form.first && this.form.last;
    },
  },

  methods: {
    recebiSalvar(index) {
      if (this.tarefas[index].status == "pending") {
        this.tarefas[index].status = "complete";
      }
      if (this.tarefas[index].status == "complete") {
        this.tarefas[index].status == "pending";
      }
    },
    deletarTarefa(tarefa) {
      tasksApi.removeTask(tarefa.id).then(() => {
        this.getTasks();
        this.deletado = true;
      });
    },
    mudarTarefa(tarefa) {
      tasksApi.updateTask(tarefa).then(() => {
        this.getTasks();
        this.deletado = true;
      });
    },
    getTasks() {
      TasksApi.getTasks().then((data) => {
        this.tarefas = data;
      });
    },
    createTask() {
      const novaTarefa = {
        title: this.form.first,
        project: this.form.last,
        status: "pending",
      };
      TasksApi.createTask(novaTarefa).then(() => {
        this.getTasks();
        this.snackbar = true;
      });
    },
  },
  created() {
    this.getTasks();
  },
};
</script>
