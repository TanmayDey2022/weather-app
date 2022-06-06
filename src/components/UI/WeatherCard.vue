<template>
  <div class="card">
    <span class="icon"
      ><img
        class="img-fluid"
        src="https://img.icons8.com/emoji/96/000000/sun-emoji.png"
    /></span>
    <div class="title"><p>Sydney</p></div>
    <div class="temp">+20<sup>&deg;</sup></div>
  </div>
</template>
<style scoped>
.card {
  padding: 1rem;
  border-radius: 1rem;
  border-color: rgba(238, 237, 237, 0.22);
  box-shadow: 5px 6px 6px 2px #e9ecef;
  text-align: center;
}

.card:hover {
  cursor: pointer;
  background-image: linear-gradient(to bottom right, #9cbdff, #6498ff);
  color: #fff;
}

.title {
  justify-content: center;
  position: absolute;
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%);
}
.title p {
  margin-bottom: 0.2rem;
  font-size: 1.2rem;
}
.temp {
  font-size: 2.6rem;
  margin-bottom: 1rem;
}
.header {
  color: #dddddd;
}
.icon {
  align-self: flex-end;
  margin-right: -2.5rem;
  margin-top: -2rem;
  z-index: 999;
}
.icon img {
  width: 5rem;
}
</style>
