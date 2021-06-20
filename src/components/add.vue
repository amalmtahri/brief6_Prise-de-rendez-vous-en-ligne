<template>
    <div class="container add ">
     <h1 class="text-center">Rendez Vous</h1>
     <form action="">
     <div class="shadow p-3 mb-5 bg-white rounded">
     <div class="form-group mb-3 col-10 ms-5 pt-2">
         <label for="form-label">Date :</label>
         <input type="date" v-model="date" class="form-control">
     </div>
     <div class="form-group mb-3 col-10 ms-5 pt-2">
        <label for="form-label">Horaire :</label>
        <select class="form-control" v-model="horaire" >
                <option></option>
                <option value="8-10">8-10</option>
                <option value="10-12">10-12</option>
                <option value="14-16">14-16</option>
                <option value="16-18">16-18</option>
        </select>
    </div>
    <div class="form-group mb-3 col-10 ms-5 pt-2">
        <label for="form-label">Type Consultation :</label>
       <textarea name="" id="" class="form-control" v-model="typeconsult" cols="30" rows="10"></textarea>
    </div>
    <div class="mb-3 col-10 ms-5 justify-content-center">
        <button class="btn "  v-on:click.prevent="Add()" type="submit">Enregistrer</button>
    </div>
</div>
</form>
 </div>
</template>

<script>
	
export default {
	name:'add',
	data(){
		return{
			date:'',
			horaire:'',
			typeconsult:'',
            reference: this.$route.params.reference
			
		}
		
	},
    methods: {
        Add(){
            	fetch("http://localhost/brief6/Rendezvous/create", {
				method: "POST",
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({
					date: this.date,
                    horaire: this.horaire,
                    typeconsult: this.typeconsult,
                    reference :this.reference
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
label{
    color: #407088;
}
button{
    background-color: #407088;
    color: white;

}
button:hover{
    background-color: #4A829E;
}
.add{
    width: 50%;
    border-radius: 20px;
    margin-top: 20px;
    box-shadow: 8px 7px 7px #EEEEEE;
}
h1{
    color: #407088;
}

</style>