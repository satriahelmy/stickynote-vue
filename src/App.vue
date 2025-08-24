<script setup>
import { ref } from 'vue';

const showForm = ref(false);
const newMemo = ref("");
const memos = ref([]);
const errorMessage = ref("");

function addMemo(){
  memos.value.push({
    id: Date.now(),
    memo : newMemo.value,
    date: new Date().toLocaleDateString(),
    backgroundColor: `#${Math.floor(Math.random()*16777215).toString(16)}`
  });

  newMemo.value = "";
  showForm.value = false;
}

function deleteMemo(id){
  memos.value = memos.value.filter(memo => memo.id !== id);
}

</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button class="header-button" @click="showForm = true">+</button>
      </header>
      <div class="card-container">
        <div v-for="memo in memos" :key="memo.id" class="card" :style="{ backgroundColor: memo.backgroundColor }">
          <p class="card-content">{{memo.memo}}</p>
          <div class="card-footer">
            <p class="card-date">{{memo.date}}</p>
            <button @click="deleteMemo(memo.id)" class="card-button">X</button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <button @click="showForm = false" class="form-close-btn">
           &times; 
        </button>
        <textarea v-model="newMemo" name="memo" id="memo" cols="30" rows="10" class="form-textarea"></textarea>
        <button @click="addMemo" class="form-save-btn">Save</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
main{
  height: 100vh;
  width:100vw;
}

.container{
  max-width: 900px;
  padding: 10px;
  margin:0 auto;
}

.card-content{
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: white;
}

.card-date{
  font-size: 12px;
  color: white;
  text-align: right;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.card-footer{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title{
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: #495a7d;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header-button{
  background-color: #495a7d;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px 15px;
  cursor: pointer;
}

.header-button:hover{
  background-color: #3b4a6b;
}

.card-button{
  background-color: #ff4d4d;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
}

.card-container{
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
}

.card{
  width: 225px;
  height:225px;
  background-color: #ffa6c1;
  border: 1px solid #eee;
  border-radius: 5px;
  padding: 15px;
  margin-bottom: 15px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.form-overlay{
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width:100%;
  height: 100%;
  z-index: 10;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal{
  background-color: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  width: 420px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-save-btn{
  background-color: #495a7d;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 10px 15px;
  cursor: pointer;
  font-size: 20px;
  width: 100%;
  margin-top: 15px;
}

.form-save-btn:hover{
  background-color: #3b4a6b;
}

.form-close-btn{
  width: 30px;
  height: 30px;
  background-color: transparent;
  background: none;
  border: none;
  font-size: 15px;
  cursor: pointer;
  position: absolute;
  top: 5px;
  right: 10px;
}

textarea{
  width: 95%;
  height: 150px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  font-size: 16px;
  resize: none;
}

.form-error{
  color: red;
  margin-bottom: 10px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

</style>