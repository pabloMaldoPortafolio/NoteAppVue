<script setup>
import {ref} from "vue"

const showModal = ref(false); 
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref("");

function getRandomColor() {
  return  "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if(newNote.value.length < 5){
    return errorMessage.value = "Note needs 5 or more characters! Try again."
  }
  notes.value.push({
    id: Math.floor(Math.random()*1000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
}


</script>


<template>
  <main>

    <div v-if="showModal" class="overlay">
      <div class="modal">
        <h1>Add Note Here</h1>
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage" > {{ errorMessage }}</p>
        <button @click="addNote">
          add Note
        </button>
        <button @click="showModal = false" class="close">
          close
        </button>
      </div>
    </div>

    <div class="container">
      <header> 
        <h1>
          Notes
        </h1>
        <button @click="showModal = true">
          +
        </button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :style= "{backgroundColor: note.backgroundColor}"
        class="card"  :key="note.id"  >
          <p class="main-text"> {{ note.text }}</p>
          <p class="date"> {{ note.date.toLocaleDateString("en-US") }}</p>

        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;

}

header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 80px;
}
header button{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21,20,20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}
.card{
  width: 225px;
  height: 225px;
  background-color: rgb(237,182,44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}
.date{
  font-size: 12px;
  font-weight: bold;
}

.cards-container{
display: flex;
flex-wrap: wrap;
}
.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0,0,0,0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal{
width: 750px;
background-color: white;
border-radius: 10px;
padding: 30px;
position: relative;
display: flex;
flex-direction: column;
}

.modal button{
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close{
  background-color: red;
  margin-top: 7px;
}
.modal p {
  color: red;
}
</style>