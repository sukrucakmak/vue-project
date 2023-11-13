<script setup>
import { ref } from "vue"

const showModal = ref(false)
const newNote = ref("")
const notes = ref([])
const errorMessage = ref("")

const addNote = () => {
  if (newNote.value.trim().length < 10) {
    errorMessage.value = "Note must be 10 characters or more"
    return errorMessage
  }

  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: "hsl(" + Math.random() * 360 + ", 100%, 75%)"
  })
  showModal.value = false
  newNote.value = ""
  errorMessage.value = ""
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage" class="errorMessage">{{ errorMessage }}</p>
        <button class="add-note" @click="addNote">Add Note</button>
        <button class="close-note" @click="showModal = false">Close</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="open-modal" @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="note in notes" :key="note.id" :style="{ backgroundColor: note.backgroundColor }">
          <p class="card-text">{{ note.text }}</p>
          <p class="card-date">{{ note.date.toLocaleDateString("tr") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  width: 100vw;
  height: 100vh;
}

.container {
  max-width: 780px;
  padding: 16px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  font-size: 4rem;

}

.open-modal {
  border: none;
  padding: 0.5rem;
  width: 3rem;
  height: 3rem;
  cursor: pointer;
  background-color: black;
  border-radius: 100%;
  color: aliceblue;
  font-size: 2rem;
}

.card {
  width: 220px;
  height: 220px;
  border-radius: 1rem;
  background-color: bisque;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  z-index: 10;
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.75);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 600px;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  color: white;
  font-size: 1.5rem;
  border: none;
  cursor: pointer;
  margin-top: 0.5rem;
  background-color: blueviolet;
}


.modal .close-note {
  margin-top: 0.25rem;
  background-color: rgb(176, 19, 19);
}

.modal .errorMessage {
  color: rgb(176, 19, 19);
}
</style>