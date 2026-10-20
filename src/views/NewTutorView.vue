<script setup>

import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); //chama router
const API_URL = 'http://localhost:3000'; //chama api

const tutores = ref([]);


async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);
  tutores.value = await resposta.json();
  console.table(tutores.value);
}

const novoTutor = ref({
  nome: '',
  telefone: '',
});

async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });

  router.push('/pets/tutores');

}
onMounted(carregarTutores)
</script>
<template>

    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de tutores</h1>
    </header>

    <RouterLink
     class="btn btn-primary"
      :to="{ name: 'novo-tutores-pet' }"
    >
      Adicionar Tutor
    </RouterLink>
    <form
      class="row"
      @submit.prevent="salvarTutor"
    >
      <div class="col-md-6">
          <div class="class">
        <label
          for="nome"
          class="form-label"
        >
          Nome Completo:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoTutor.nome"
          required
        />

      </div>
      </div>

     <div class="col-md-6">
          <div class="class">
        <label
          for="telefone"
          class="form-label"
        >
          telefone/whatsapp:
        </label>
        <input
          type="tel"
          class="form-control"
          id="telefone"
          v-model="novoTutor.telefone"
          required
        />

      </div>


      </div>
      <div class="col-12 d-flex gap-2">
        <button
          class="btn btn-success mt-2"
          type="submit"
        >
          Salvar Tutor
        </button>
      </div>
    </form>


</template>

