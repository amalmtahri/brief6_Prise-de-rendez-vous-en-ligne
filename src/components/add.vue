<template>
    <div class="container add ">
     <h1 class="text-center">Rendez Vous</h1>
     <div class="alert alert-danger" role="alert" v-if="erreur">
                {{ erreur }}
            </div>
     <form action="">
     <div class="shadow p-3 mb-5 bg-white rounded">
     <div class="form-group mb-3 col-10 ms-5 pt-2">
         <label for="form-label">Date :</label>
         <input type="date" v-model="date" class="form-control">
     </div>
     <div class="form-group mb-3 col-10 ms-5 pt-2">
        <label for="form-label">Horaire :</label>
        <select class="form-control" v-model="horaire" >
               <option selected disabled>choisir un horaire</option>
                        <option v-for="(duree,index) in DureesS" :key="index" :disabled="duree.etat">{{ duree.val }}</option>
                        
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
			horaire:'choisir un horaire',
			typeconsult:'',
            reference: this.$route.params.reference,
            DureesS: [
                { val: "08:00-09-00", etat: false },
                { val: "09:00-10-00", etat: false },
                { val: "10:00-11-00", etat: false },
                { val: "11:00-12-00", etat: false },
                { val: "13:00-14-00", etat: false },
                { val: "14:00-15-00", etat: false },
                { val: "15:00-16-00", etat: false },
                { val: "16:00-17-00", etat: false },
                { val: "17:00-18-00", etat: false },
            ],
            Durees: [],
            erreur: "",
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

        },
       async getTimes(dateP){
           console.log("rrrrr");
            const response= await fetch("http://localhost/brief6/Rendezvous/getData/"+dateP);
           const data = await response.json();
           this.Durees=data;
           console.log(data);

        }
     
    },
    watch:{
        date: async function(val){
            await this.getTimes(val);
            if(this.DureesS.length == this.Durees.length){
                this.erreur="il reste plus de rdv pour cette date";
            }
            else{
                 this.erreur="";
            }
            this.horaire="choisir un horaire";
            for (var i = 0; i < this.DureesS.length; i++) {
                    this.DureesS[i].etat = false;
                    for (var j = 0; j < this.Durees.length; j++) {
                        if (this.DureesS[i].val == this.Durees[j].horaire) {
                            console.log(this.DureesS[i].etat);
                            this.DureesS[i].etat = true;
                            console.log(i + "///eg///" + this.DureesS[i].etat);
                            // break;
                        }
                    }
                }
            


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