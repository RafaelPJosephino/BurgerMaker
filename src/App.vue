<template>
  <Navbar :logo="src_logo" :alt="app_name"/>
  <router-view class="main-container"/>
  <Footer/>
</template>
<script>
import Navbar from "./components/Navbar.vue"
import Footer from "./components/Footer.vue";
export default {
components:{
  Navbar,
  Footer
},
data(){
  return{
    src_logo: "/img/logo.png",
    app_name: "Faca seu hamburguer"
  }
}

}
</script>

<style>
*{
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;

}
.main-container{
margin: 50px;
min-height: 250px;

}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
