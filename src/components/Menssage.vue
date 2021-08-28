<template>
    <div class="message-container">
        <p>{{msg}}</p>
    </div>
</template>
<script>
export default {
    name:"Mensagem" ,
    props:{
        msg : String,
    }   

}
</script>
<style>
.message-container {
color: rgb(2, 80, 2);
background-color:rgb(49, 238, 49) ;
border: 2px solid rgb(119, 119, 119);
border-radius: 5px;
padding: 10px;
max-width: 400px ;
margin: 30px auto;
}
</style>