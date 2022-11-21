<template>
  <div class="statement-saved-modal-overlay" @click="$emit('close-modal')">
    <div class="statement-saved-modal">
<!--      <img class="check" src="~/assets/check-icon.png" alt="" />-->
      <h6>Сохранено!</h6>
    </div>
<!--    <div class="close"  @click="$emit('close-modal')">-->
<!--      <img class="close-img" src="@/assets/logo.png" alt="" />-->
<!--    </div>-->
  </div>
</template>

<script>
export default {
}
</script>

<style scoped>

.statement-saved-modal-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  background-color: #000000da;
}

.statement-saved-modal {
  text-align: center;
  background-color: white;
  height: 70px;
  width: 300px;
  margin-top: 10%;
  padding: 60px 0;
  border-radius: 20px;
}
.close {
  margin: 10% 0 0 16px;
  cursor: pointer;
}

.close-img {
  width: 25px;
}

.check {
  width: 150px;
}

h6 {
  font-weight: 500;
  font-size: 28px;
  margin: 20px 0;
}

p {
  font-size: 16px;
  margin: 20px 0;
}

button {
  background-color: #ac003e;
  width: 150px;
  height: 40px;
  color: white;
  font-size: 14px;
  border-radius: 16px;
  margin-top: 50px;
}
</style>