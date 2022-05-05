<template>
  <div class="header_wrapper">
    <div class="company_logo_container"><img class="company_logo" src="@/assets/logo.png" alt="logo">
    </div>
    <h1 class="header-title"> A simple blog </h1>
    <nav class="nav_bar">
      <ul class="nav_bar_item_container">
        <li class="nav_bar_items">Ru</li>
        <li class="nav_bar_items">En</li>
      </ul>
    </nav>
  </div>
</template>

<script>

export default {
  name: "Header",
}

</script>

<style scoped>
.header_wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 15px 0;
  background-color: teal;
  border-radius: 2px;
}
.company_logo {
  height: 50px;
  width: 50px;
}
.company_logo:hover {
  cursor: pointer;
}
.nav_bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 60px;
  color: wheat;
}
.nav_bar_item_container {
  width: 50px;
  display: flex;
  justify-content: space-between;
  list-style-type: none;
  font-weight: bold;
}
.nav_bar_items:hover{
  cursor: pointer;
}
.header-title {
  color: wheat;
  font-family: "Roboto";
  text-transform: capitalize;
}

</style>