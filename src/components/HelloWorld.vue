<script setup>
import { ref } from 'vue';

const languaje = ref(null)
const programmers = ref(null)

defineProps({
  msg: {
    type: String,
    required: true
  }
})

async function sendDatos(){

  const info = {
    name: languaje.value,
    programmers: programmers.value
  }
  try {
        await fetch('http://localhost:3000/api/languaje', {
            method: "POST",
            headers: { "Content-type": "application/json;charset=UTF-8" },
            body: JSON.stringify(info)
        })
            .then(console.log(info)) 
    } catch (error) {
        console.log(error);
    }
  location.reload()
}
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
      <input v-model="languaje" type="text" class="form-control my-4" placeholder="LENGUAJE" />
      <input v-model="programmers" type="number" class="form-control my-4" placeholder="N° PROGRAMADORES" />
      <button @click="sendDatos" class="btn btn-primary">Enviar</button>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
