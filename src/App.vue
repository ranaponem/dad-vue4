<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

// persons is the reactive data that will store the persons data loaded from the API
const persons = ref([])
const API_URL = 'https://fakerapi.it/api/v2/persons?_locale=pt_PT&_quantity=5'

const loadDataWithFetch = () => {
  fetch(API_URL)
    .then(response => response.json())
    .then(responseJson => persons.value = responseJson);
}

const asyncLoadDataWithFetch = async () => {
  const response = await fetch(API_URL)
  const responseJson = await response.json()
  persons.value = responseJson
}

const loadDataWithAxios = () => {
  axios.get(API_URL)
    .then((response) => {
      persons.value = response.data
    })
    .catch((error) => {
      // handle error
      console.log(error)
    })
    .finally(() => {
      // always executed
    })
}

const asyncLoadDataWithAxios = async () => {
  try {
    const response = await axios.get(API_URL)
    persons.value = response.data
  } catch (error) {
    // handle error
    console.error(error)
  } finally {
  // /always executed
}
}

const loadData = () => {
  // Try 4 different ways to fetch data:
  // loadDataWithFetch()
  // asyncLoadDataWithFetch()
  // loadDataWithAxios()
  asyncLoadDataWithAxios()
}
onMounted(() => {
  // Load data when the component is mounted
  loadData()
})


</script>

<template>
  <div class="p-8 mx-auto max-w-3xl" id="app">
    <h1 class="pb-4 text-5xl">Persons</h1>
    <h2 class="py-2 text-lg">Consumes data from the public API "FakerAPI"</h2>
    <p class="pb-4 text-sm underline text-blue-600"><a href="https://fakerapi.it/">https://fakerapi.it/</a></p>
    <div>
      <div class="py-4">
        <button type="button" class="w-40 h-10 shrink-0 inline-flex justify-center items-center gap-x-2 
                      text-base rounded-md bg-blue-600 text-white 
                      border border-transparent 
                      hover:bg-blue-700 focus:outline-none focus:bg-blue-700" @click="loadData">Load Data</button>
      </div>
      <div class="mt-4">
        <table class="table-auto border-collapse border border-slate-400 text-left">
          <thead>
            <tr>
              <th class="p-3 border border-slate-300 bg-gray-200">Name</th>
              <th class="p-3 border border-slate-300 bg-gray-200">Date of Birth</th>
              <th class="p-3 border border-slate-300 bg-gray-200">Phone</th>
              <th class="p-3 border border-slate-300 bg-gray-200">City</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="person in persons.data">
              <td class="py-2 px-3 border border-slate-300">{{ person.firstname + ' ' + person.lastname }}</td>
              <td class="py-2 px-3 border border-slate-300">{{ person.birthday }}</td>
              <td class="py-2 px-3 border border-slate-300">{{ person.phone }}</td>
              <td class="py-2 px-3 border border-slate-300">{{ person.address.city }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
