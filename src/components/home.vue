<template>
    <div class=" home row justify-content-center">
    <router-link class="btn accueil" to="/reference">Rendez-vous</router-link>&nbsp;&nbsp;
        <router-link class="btn accueil" to="/register">Nouveau Utilisateur</router-link> 
        
    </div>
    
</template>

<script>
export default {

}
</script>

<style>

.home{
    margin: 0;
    padding: 0;
    background-color: #41AEA9;  
    height:100vh;
}
.accueil{
    background-color: #407088;
    color: white;
    width: 200px;
    height: 40px;
   
}
.accueil:hover{
    background-color: #4A829E;
    color:white;
}
a{
    text-decoration: none;
    color: white;
    margin-top: 25%;
   
}
</style>