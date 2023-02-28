<script setup>
  import {ref} from "vue";

  const showModal = ref(false);
  const newNote = ref("");
  const notes = ref([]);
  const errorMessage = ref("");

  function getRandomColor()
  {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () =>
  {
    if (newNote.value.length > 10)
    {
      notes.value.push(
      {
        id: Math.floor(Math.random() * 1000000),
        text: newNote.value,
        date: new Date(),
        backgroundColor: getRandomColor()
      });
      showModal.value = false;
      newNote.value = "";
      errorMessage.value = "";
    }
    else
      errorMessage.value = "Please enter a minimum of 9 characters to create a note!";
  }

  const close = () =>
  {
    showModal.value = false;
    newNote.value = "";
    errorMessage.value = "";
  }
</script>

<template>
  <main>
    <!-- Nếu showModal = true thì sẽ render phần overlay -->
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <!-- Kết nối textarea với newNote để dữ liệu luôn được update -->
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <!-- Nếu errorMessage != "" thì sẽ hiện thông báo lỗi -->
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote()">Add note</button>
        <button class="close" @click="close()">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <!-- Duyệt qua các phần tử trong mảng notes và tạo thẻ div tương ứng -->
        <!-- Thêm key để xóa phần tử -->
        <div v-for="note in notes" class="card" :style="{backgroundColor: note.backgroundColor}" :key="note.id">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("vi-VN") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main
  {
    width: 100vw;
    height: 100vh;
  }

  .container
  {
    max-width: 1000px;
    padding: 10px;
    margin: auto;
  }

  header
  {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1
  {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  header button
  {
    border: 0px;
    padding: 10px;
    width: 50px;
    height: 50px;
    border-radius: 100%;
    background-color: antiquewhite;
    font-size: 30px;
  }

  .card
  {
    width: 225px;
    height: 225px;
    background-color: khaki;
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .main-text
  {
    color: black;
  }

  .date
  {
    color: black;
    font-size: 13px;
    font-weight: bold;
  }

  .cards-container
  {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay
  {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal
  {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button
  {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    color: white;
    border: 0px;
    cursor: pointer;
    margin-top: 15px;
  }

  .modal .close
  {
    background-color: rgb(250, 86, 86);
    margin-top: 7px;
  }

  .modal p
  {
    color: rgb(248, 67, 67);
  }
</style>