<template>
   <div class="container register">
        <form action="">
        <div class="justify-content-center row">
          
            <h1>Inscription</h1>
             <div class="form-group mb-3 col-10 ms-5 pt-2">
                <label for="form-label">Reference :</label>
                <input type="text" v-model="reference" class="form-control" required>
            </div>
           
            <div class="form-group mb-3 col-10 ms-5 pt-2">
                <label for="form-label">Nom :</label>
                <input type="text" v-model="nom" class="form-control" required>
            </div>
            <div class="form-group mb-3 col-10 ms-5 pt-2">
                <label for="form-label">Prénom :</label>
                <input type="text" v-model="prenom" class="form-control" required>
            </div>
            <div class="form-group mb-3 col-10 ms-5 pt-2">
                <label for="form-label">Tél :</label>
                <input type="text" v-model="tele" class="form-control" required>
            </div>
            <div class="form-group mb-3 col-10 ms-5 pt-2">
                <label for="form-label">Age :</label>
                <input type="number" v-model="age" class="form-control" required>
            </div>
            
            <div class="mb-3 col-10 ms-5 ">
              <button class="btn " type="submit" v-on:click.prevent="register()">Send</button>
            </div>
       
        </div>

    </form>
       

    
    </div>
</template>

<script>
	
export default {
	name:'register',
	data(){
		return{
            reference:'',
			nom:'',
			prenom:'',
			tele:'',
			age:'',
		}
		
	},
	methods: {
		register(){
			fetch("http://localhost/brief6/Register/create", {
				method: "POST",
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
                    reference: this.reference,
					nom: this.nom,
					prenom: this.prenom,	
					tel:this.tele,
                    age:this.age
				})
			})
			.then(() => {
		
				 this.$router.push("/Rendezvous/"+this.reference)
				
			})
			.catch(function(err){
				console.log(err);
			})
		}
	}
	

	
}
</script>

<style>
body{
    margin: 0;
    padding: 0;
   
}
h1{
    margin-top:40px;
    color: #407088;
}

button{
    background-color: #407088;
    color: white;

}
button:hover{
    background-color: #4A829E;
}
label{
    color: #407088;
}
</style>