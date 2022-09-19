<template>
  <div>
    <v-list-item
      @click="salvarClique"
      :input-value="tarefa.status == 'complete' ? true : false"
      :class="tarefa.status == 'complete' ? 'teal lighten-4' : 'white'"
    >
      <template v-slot:default="{}">
        <v-list-item-action>
          <v-checkbox
            :input-value="tarefa.status == 'complete' ? true : false"
          ></v-checkbox>
        </v-list-item-action>

        <v-list-item-content>
          <v-list-item-title>{{ tarefa.title }}</v-list-item-title>
          <v-list-item-subtitle>{{ tarefa.project }}</v-list-item-subtitle>
        </v-list-item-content>

        <v-btn @click="mandarDeletar" depressed color="error"> Deletar </v-btn>
      </template>
    </v-list-item>
  </div>
</template>

<script>
export default {
  props: ["tarefa"],
  methods: {
    salvarClique() {
      this.$emit("salvarClick");
    },
    mandarDeletar() {
      this.$emit("mandarDel");
    },
  },
};
</script>
