<template>
  <div id="app">
    <Header/>
    <HomePage/>
    <Footer/>
  </div>
</template>

<script>
import Header from "@/components/Header/Header";
import Footer from "@/components/Footer/Footer";
import HomePage from "@/pages/HomePage";

export default {
  name: 'App',
  components: {
    HomePage,
    Header,
    Footer,
  },

}
</script>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
button {
  min-height: 20px;
  padding: 2px;
  border: 1px solid teal;
  background-color: aliceblue;
  color: teal;

}
#app {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  width: 1200px;
  min-height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: lightgrey;
}
</style>
