<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';
const tutor = ref(null);
const pets = ref([]);
const petTutor = ref([]);

const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
});
async function carregarTutores() {
  const idTutor = route.params.id;

  const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
  console.log('respostaTutor', respostaTutor);
  tutor.value = await respostaTutor.json();
  console.log('tutor', tutor.value);

  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();

  petTutor.value = pets.value.filter(
        pet => pet.tutorId === tutor.value.id
    );

    novoPet.value.tutorId = tutor.value.id
}

async function salvarPet() {
  await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),
  });

  await carregarTutores();

  novoPet.value.nome = '';
  novoPet.value.especie = '';
  
}


onMounted(carregarTutores);
</script>

<template>
  <section v-if="tutor == null">
    <p>carego</p>
  </section>

  <section v-else>
    <h1>Nome do tutor: {{ tutor.nome }}</h1>
    <p> Telefone: {{ tutor.telefone }}</p>

    <div>
    <p class="text-body-secondary mb-0">Pets do Tutor:</p>
    <div>
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th>id</th>
              <th>Nome</th>
              <th>Espécie</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="pet in petTutor"
              :key="pet.id"
            >
              <td>{{ pet.id }}</td>
              <td>{{ pet.nome }}</td>
              <td>{{ pet.especie }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
       <div>
      <form
        class="row"
        @submit.prevent="salvarPet"
      >
        <div class="col-md-6">
          <label
            for="nome"
            class="form-label"
          >
            Nome do Pet:
          </label>

          <input
            type="text"
            class="form-control"
            id="nome"
            v-model="novoPet.nome"
            required
          />
        </div>

        <div class="col-md-6">
          <label
            class="form-label"
            for="especie"
            >Espécie</label
          >
          <select
            name="especie"
            id="especie"
            class="form-select"
            required
            v-model="novoPet.especie"
          >
            <option
              value=""
              disabled
            >
              Selecione a espécie:
            </option>
            <option value="Cachorro">Cachorro</option>
            <option value="Gato">Gato</option>
            <option value="Coelho">Coelho</option>
            <option value="Tartaruga">Tartaruga</option>
          </select>
        </div>

        <div class="col-12 d-flex gap-2 mt-3">
          <button
            class="btn btn-success"
            type="submit"
          >
            Salvar Pet
          </button>
        </div>
      </form>
    </div>
  </section>
</template>
