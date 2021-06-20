<template>
  <div class="container reference">
        <form action="" v-on:submit.prevent="onsub">
        <div class="form-group mb-3 col-10 ms-5 pt-2">
            <label for="from-label">Reference :</label>
            <input type="text" v-model="reference" class="form-control">
        </div>
        <div class="mb-3 col-10 ms-5 justify-content-center">
            <button class="btn ref" name="submit" type="submit">Login</button> 
        </div>
    </form>
    </div>
</template>

<script>
	
export default {
	name:'reference',
	data(){
		return{
			reference:''
		}
		
	},
    methods: {
        
        onsub(){
            fetch("http://localhost/brief6/Register/afficher/"+this.reference,{
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                }
            })
            .then((res) => {
             return res.json();
        })
            .then((res) => {
                if(res==1){
                this.$router.push("/Rendezvous/"+this.reference);
                }else{
                alert('Reference incorrecte');
            }
            console.log(res);
            })
            .catch((err) => {
             console.log(err);
            }); 
        }
    },
    mounted:function(){
        
    }
	
}
</script>

<style>
.reference{
    width: 50%;
    border-radius: 20px;
    
    box-shadow: 8px 7px 7px #EEEEEE;
    padding-bottom: 30px;
    margin-top: 15%;
}
.ref{
    background-color: #407088;
    color: white;

}
.ref:hover{
    background-color: #4A829E;
}
label{
    color: #407088;
}
</style>