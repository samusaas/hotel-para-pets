<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink,useRouter } from 'vue-router';

const router = useRouter(); //chama router
const API_URL = 'http://localhost:3000'; //chama api

const tutores = ref([]);

// criar obj para salvar novoPet
//nome, espécie e tutorId

const novoPet = ref({
    nome: '',
    especie: '',
    tutorId: ''
});

//buscar tutores salvos

async function carregarTutores() {
    const resposta = await fetch(`${API_URL}/tutores`);
    tutores.value = await resposta.json();
    console.table(tutores.value)
}

    async function salvarPet() {
        await fetch(`${API_URL}/pets`, {
            method: 'POST',
            headers: {
                'Content-type': 'application/json',
            },
            body: JSON.stringify(novoPet.value)
        })

        router.push('/pets');
    }

onMounted(carregarTutores)
</script>
<template>
    <div>

        <header class="mb-4">
            <h1 class="text-2xl font-bold">Listagem de pets</h1>
            <p class="text-body-secondary mb-0">
                Listagem dos Pets cadastrados no sistema
            </p>
        </header>
        <RouterLink class="btn btn-primary" :to="{ name: 'novo-pet'}">
            Adicionar Pet
        </RouterLink>
        <form class="row" @submit.prevent="salvarPet">

            <div class="col-md-6"></div>
            
            <div class="col-md-6"></div>
         
            <div class="col-md-6">
            <label for="tutor" class="form-label">Tutor</label>
            <select name="tutor" id="tutor" v-model="novoPet.tutorId">
                <option value="" disabled>Selecione o tutor</option>
                <option v-for="tutor in tutores" :key="tutor.id">{{ tutor.nome }}</option>
            </select>
         </div>

            <div class="class">
                <label for="nome" class="form-label">
                    Nome do pet:
                </label>
                <input 
                type="text"
                class="form-control"
                id="nome"
                v-model="novoPet.nome"
                required
                >

            <label class="form-label" for="especie">Especie</label>
            <select name="especie" id="especie" class="form-select" required v-model="novoPet.especie">
            <option value="" disabled> Selecione a especie:</option>
            <option value="Cachorro"></option>
            <option value="Gato"></option>
            <option value="Coelho"></option>
        </select> 
       </div>
       <div class="col-12 d-flex gap-2">
        <button class="button btn-sucess" type="submit">Salvar Pet</button>
       </div>
        </form>        

    </div>
</template>