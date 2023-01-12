<script setup>
import { onMounted, ref } from 'vue';

const data = ref(null)
const updateID = ref(null)
const languaje = ref(null)
const programmers = ref(null)
async function datos(){
  try {
        await fetch('http://localhost:3000/api/languaje', {
            method: "GET",
            headers: { "Content-type": "application/json;charset=UTF-8" }
        })
            .then(response => response.json())  // convertir a json
            .then(json => data.value = json)    //imprimir los datos en la consola
        console.log(data.value);
    } catch (error) {
        console.log(error);
    }
}

async function borrar(id){
  console.log(id);
  try {
    await fetch(`http://localhost:3000/api/languaje/${id}`, {
      method: "DELETE",
      headers: {
        'Content-type': 'application/json'
      }
    })
    location.reload()
  } catch (error) {
    console.log(error);
  }
}

function update(id){
  updateID.value = id
  console.log(updateID.value);
}

async function actualizar(){
  const info = {
    name: languaje.value,
    programmers: programmers.value
  }
  
  try {
    await fetch(`http://localhost:3000/api/languaje/${updateID.value}`, {
            method: "PUT",
            headers: { "Content-type": "application/json;charset=UTF-8" },
            body: JSON.stringify(info)
        }).then(console.log(info))
        location.reload()
  } catch (error) {
    console.log(error);
  }
}

onMounted(()=>{
  datos()
})
</script>

<template>
  
  <div class="container">
    <h1>LISTA</h1>

    <div>
      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">#ID</th>
            <th scope="col">Lenguaje</th>
            <th scope="col">N° Programadores</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="data != null" v-for="dato in data" :key="dato.id">
            <th scope="row">{{dato.id}}</th>
            <td>{{dato.name}}</td>
            <td class="text-center">{{dato.programmers}}</td>
            <td>
              <button @click="update(dato.id)" type="button" class="btn btn-info mx-2" data-bs-toggle="modal" data-bs-target="#exampleModal">Actualizar</button>
              <button @click="borrar(dato.id)" type="button" class="btn btn-danger mx-2">Eliminar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>



    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Actualizar Información</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <input v-model="languaje" type="text" class="form-control my-4" placeholder="LENGUAJE" />
            <input v-model="programmers" type="number" class="form-control my-4" placeholder="N° PROGRAMADORES" />
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            <button @click="actualizar" type="button" class="btn btn-primary" data-bs-dismiss="modal">Save changes</button>
          </div>
        </div>
      </div>
    </div>



  </div>


</template>
