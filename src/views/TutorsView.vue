<script setup>
import { onMounted, ref } from 'vue';
const tutores = ref([]); // lista de tutores vazia;
const pets = ref([]);
// chamando a minha API geral:
const API_URL = 'http://localhost:3000';

async function carregarDados() {

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();
  const respostaPets = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPets.json();
}

async function excluirTutor(id){
    const confirmarExclusao = window.confirm('deseja excluir o tutor?');
  if(confirmarExclusao) {
    await fetch(`${API_URL}/tutores/${id}`,{
      method: 'DELETE',
    })
    carregarDados()

  }
}

function quantidade(tutorId) {
  return pets.value.filter(pet => pet.tutorId === tutorId).length
}


onMounted(carregarDados)
</script>
<template>
    <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de tutores</h1>
      <RouterLink
      class="btn btn-primary btn-outline"
      to="/pets/tutores/novo"
    >
      Novo tutor
    </RouterLink>
    </header>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>Id</th>
          <th>Nome</th>
          <th>Quantos pets</th>
          <th>Telefone</th>
          <th>Email</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>

        <tr v-for="tutor in tutores"
        :key="tutor.id">
        <td> {{ tutor.id }}</td>
        <td> {{ tutor.nome }}</td>
        <td>{{ quantidade(tutor.id) }}</td>
        <td> {{ tutor.telefone }}</td>
        <td> {{ tutor.email }}</td>
        <td> <RouterLink :to="`/pets/tutores/${tutor.id}`" >visualizar </RouterLink></td>
        <td> <button class="btn btn-danger" @click="excluirTutor(tutor.id)">Excluir</button></td>
        </tr>


         <!-- <tr
          v-for="pet in pets"
          :key="pet.id"
        >
          <td>{{ pet.id }}</td>
          <td>{{ pet.nome }}</td>
          <td>{{ pet.especie }}</td>
          <td>{{ nomeDoTutor(pet.tutorId) }}</td>
          <td> <RouterLink :to="`/pets/${pet.id}`" >visualizar </RouterLink></td>
          <td>
            <button class="btn btn-danger" @click="excluirPet(pet.id)">Excluir</button>
          </td>
        </tr> -->
      </tbody>
    </table>
    </div>
</template>
