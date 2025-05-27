<template>
  <div>
    <h1>Lista de Contactos</h1>

    <button @click="addRandomContact">Añadir contacto aleatorio</button>
    <button @click="sortByName">Ordenar por Nombre</button>
    <button @click="sortByPopularity">Ordenar por Popularidad</button>

    <table>
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won<br>Oscar</th>
          <th>Won<br>Emmy</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(contact, index) in contacts" :key="contact.id">
          <td>
            <img :src="contact.pictureUrl" :alt="contact.name" width="80" />
          </td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>{{ contact.wonOscar ? '🏆' : '' }}</td>
          <td>{{ contact.wonEmmy ? '🌟' : '' }}</td>
          <td>
            <button @click="deleteContact(contact.id)">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import allContacts from './contacts.json'

const contacts = ref(allContacts.slice(0, 5))

function addRandomContact() {
  const remainingContacts = allContacts.filter(
    contact => !contacts.value.some(c => c.id === contact.id)
  )

  if (remainingContacts.length === 0) {
    alert('No quedan más contactos para añadir.')
    return
  }

  const randomIndex = Math.floor(Math.random() * remainingContacts.length)
  contacts.value.push(remainingContacts[randomIndex])
}

function sortByName() {
  contacts.value = [...contacts.value].sort((a, b) =>
    a.name.localeCompare(b.name)
  )
}

function sortByPopularity() {
  contacts.value = [...contacts.value].sort((a, b) => b.popularity - a.popularity)
}

function deleteContact(id) {
  contacts.value = contacts.value.filter(contact => contact.id !== id)
}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #fdf6f0;
  color: #333;
}

h1 {
  text-align: center;
  margin: 40px 0 20px;
  font-size: 2.5em;
  color: black;
  text-shadow: 1px 1px 2px #999;
}

table {
  width: 90%;
  margin: 0 auto;
  border-collapse: separate;
  border-spacing: 0 10px;
}

thead th {
  background-color: #e74c3c;
  color: white;
  padding: 15px;
  border-radius: 8px 8px 0 0;
  font-size: 16px;
}

tbody td {
  background-color: white;
  padding: 15px;
  border-bottom: 1px solid #ddd;
  border-top: 1px solid #eee;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
  text-align: center;
  vertical-align: middle;
}

tbody tr {
  transition: transform 0.2s;
}

tbody tr:hover {
  transform: scale(1.01);
}

img {
  border-radius: 10px;
  box-shadow: 0 0 5px rgba(0,0,0,0.2);
}

button {
  background-color: #c0392b;
  color: white;
  border: none;
  padding: 8px 15px;
  margin: 5px;
  border-radius: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #e74c3c;
}

#app {
  padding-bottom: 50px;
}
</style>

