<script setup>
import { ref, watch } from "vue"

const showModal = ref(false)
const newNote = ref("")
const notes = ref([])
const error = ref()
const colorPicker = ref("")

const selectedColor = () => {
  if (colorPicker.value == "#000000")
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  else {
    return colorPicker.value
  }
}

const shortText = (text) => {
  if (text.length > 200) {
    const result = text.slice(0, 200);
    return result + " Read More..."
  }else{
    return text
  }

}

const addNote = () => {
  if (newNote.value.length > 10) {
    notes.value.push({
      "id": Math.random() * 100000,
      "text": newNote.value,
      "backgroundColor": selectedColor(),
      "date": new Date().toLocaleString()
    })

    console.log(notes.value);
    console.log(colorPicker.value);
    showModal.value = false
    newNote.value = ""
    error.value = ""
    colorPicker.value = "#000000"
  }
  else {
    error.value = "Use at least 10 character"
  }
}
watch(colorPicker, (newX) => {
  console.log(newX)
})



</script>



<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">

        <textarea v-model="newNote" minlength="10" name="note" id="note" cols="30" rows="10"></textarea>
        <p class="error" v-show="error">{{ error }}</p>
        <button @click="addNote">Add Note</button>
        <button @click="(showModal = false)" class="close">Close</button>
        <input v-model="colorPicker" type="color" id="color-picker">

      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">➕</button>
      </header>

      <div class="cards-container">

        <div v-for="note in notes">
          <div class="card" :style="{ backgroundColor: note.backgroundColor }">
            <p class="main-text">
              {{ shortText(note.text) }}
            </p>
            <p class="date">
              {{ note.date }}
            </p>
          </div>
        </div>
      </div>
    </div>

  </main>
</template>



<style scoped>
main {
  height: 100hv;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0px auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: black;
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
 overflow: hidden;
  width: 225px;
  height: 225px;
  background-color: rgb(95, 238, 190);
  border-radius: 15px;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
  margin-right: 20px;
  margin-bottom: 20px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0, 0.75);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  display: flex;
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}

.modal .close {
  margin-top: 10px;
  background-color: brown;
}

.error {
  color: brown;
  margin-top: 8px;
  margin-bottom: 0px;
}
</style>