<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button class="header-button">+</button>
      </header>
      <div>[card]</div>
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
</style>