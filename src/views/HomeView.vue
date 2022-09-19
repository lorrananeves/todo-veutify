<template>
  <v-container>
    <v-row class="text-left">
      <v-col cols="12"> </v-col>

      <v-col class="mb-2">
        <h1 class="display-2 font-weight-bold mb-3">
          Bem-vindo ao TODO Vuetify
        </h1>

        <p class="subheading font-weight-regular">
          Gerenciando tarefas aprendendo VueJS + Vuetify + API Mock
        </p>
        <p>
          <v-btn x-large rounded color="primary" dark :to="{ name: 'login' }">
            Iniciar
          </v-btn>
        </p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {};
</script>
