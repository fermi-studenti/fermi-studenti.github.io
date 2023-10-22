<script setup>
import axios from 'axios';
import { ref, onBeforeMount } from 'vue'

axios.defaults.headers.post['Content-Type'] ='application/x-www-form-urlencoded';

let username = ref("");
let content = ref("");

let reports = ref([]);

function createReport() {
  if(username.value != "" && content.value != "") {
    axios({
      method: 'post',
      url: 'https://notes-lib-2.000webhostapp.com/createReport.php',
      data: {
        author: username.value,
        content:  content.value,
      }
    }).then(() => getData());

    username.value = "";
    content.value = "";
  }
}

onBeforeMount(() => getData())

function getData() {
  axios
    .get('https://notes-lib-2.000webhostapp.com/readReports.php')
    .then((response) => {
      reports.value = response;
  })
}
</script>


<template>
  <div class="report-wrapper">
    <h1>Avete riscontrato problemi con la scuola?</h1>
    <p>Scrivetele qua e <strong>il Comitato</strong> si impegnerà a sistemarli</p>

    <div class="input-report">
      <div class="input">
        <span>Inserisci username: </span>
        <input type="text" v-model="username">
      </div>
      <div class="input">
        <span>Inserisci il problema: </span>
        <textarea type="text" v-model="content"></textarea>
      </div>

      <button @click="createReport">Invia</button>
    </div>

    <div class="report-container">

      <div class="report" v-for="report in reports.data.reverse()" :key="report.id">
        <h3>{{ report.author }}, <em>{{ report.date }}</em></h3>
        <p>{{ report.content }}</p>
      </div>
    </div>

  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter'); 

.report-wrapper {
  max-width: 50rem;
  margin: auto;
}

.input-report {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.input {
  display: flex;
  flex-direction: row;
  gap: 1rem;
}

.input span {
  width: 5rem;
  font-style: italic;
}

input, textarea {
  width: 100%;
  padding: 0.5rem;
  border-radius: 1rem;
  border: none !important;
  background-color: ghostwhite;
}

.report-container {
  margin: 2rem 0rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.report {
  background-color: ghostwhite;
  border-radius: 1rem;
  padding: 1rem;
}

* {
  font-family: 'Inter', sans-serif;
}

h1 {
  font-weight: 900;
}

button {
  border: none;
  padding: 0.8rem;
  border-radius: 1rem;
  color: white;
  background-color: lightgreen;
  font-size: 1.2rem;
  font-weight: bolder;
}
</style>
