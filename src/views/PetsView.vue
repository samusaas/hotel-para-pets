<script>
    import { onMounted, ref } from 'vue';
    import { RouterLink } from 'vue-router';

    //Minha api para listar todos os pets
    const pets = ref([]); //lista vazia
    const tutores = ([]);//lista vazia

    const API_URL = 'http://localhost:3000';

        //chamar a api para a listagem de pets e tutores
    async function carregarDados() {
        const respostaPets = await fetch (`${API_URL}/pets`)
        pets.value = await respostaPets.json();

         const respostaTutores = await fetch (`${API_URL}/tutores`)
        pets.value = await respostaTutores.json();
        console.log('PETS - ', pets.value);
        console.log('TUTORES - ', pets.value);
    }

    function nomeDoTutor(tutorId) {
        for(const tutor of tutores.value){
            if(tutor.id === tutorId){
                return tutor.nome;
            }
        }
        return 'tutor não encontrado';
    }
    onMounted(carregarDados);
</script>
<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
      <table class="table table-striped table-hover">
        <thead>
         <th>id</th>
         <th>Nome</th>
         <th>Espécie</th>
         <th>Tutor</th>
        </thead>
        <tbody>
            <tr v-for="pet in pets" :key="pet.id">
                <td>{{ pet.id }}</td>
                <td>{{ pet.nome }}</td>
                <td>{{ pet.especie }}</td>
                <td>{{ nomeDoTutor(pet.tutorId) }}</td>
            </tr>
        </tbody>
      </table>
    </header>
</div>
</template>
