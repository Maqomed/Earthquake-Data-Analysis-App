<template>
  <div class="header text-white shadow-lg">
    <div class="d-flex">
      <img src="../assets/earthquake.png" class="logo" />
      <h3>EDA - Earthquake Data Analysis </h3>
    </div>
  </div>
</template>
<script>
</script>
<style scoped>
@media only screen and (max-width: 1200px) {
  .header {
    width: 100vw;
    background-color: #563d7c;
    height: 50px;
    font-family: "Merriweather Sans", sans-serif;
  }
  .logo {
    width: 30px;
    height: 30px;
    margin-right: 10px;
  }
}
@media only screen and (min-width: 1200px) {
  .header {
    width: 100vw;
    background-color: #563d7c;
    height: 50px;
    font-family: "Merriweather Sans", sans-serif;
    padding-left: 20px;
    padding-top: 5px;
  }
  .logo {
    width: 40px;
    height: 40px;
    margin-right: 10px;
  }
}
</style>
