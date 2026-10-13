<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const API_URL = "http://localhost:3000";
const pet = ref(null);
const tutor = ref(null);

async function carregrPet() {
    const idPet = route.params.id;
    const respostaPet = await fetch(`${API_URL}/pets/{idPet}`);
    console.log('respostaPet', respostaPet);

    pet.value = await respostaPet.json();
    console.log('pet', pet.value);

    const respostaTutor = await fetch(`${pet.value.tutorId}`);
    tutor.value = await respostaTutos.json();
    console.log('tutor', tutor.value);
}

onMounted(carregrPet())
</script>
<template>
    <h1> Nome do Pet:
    </h1>
    <p>tutor:</p>
    <h5>espécie:</h5>
</template>