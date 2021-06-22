<template>
   
    <div class="container affichage">
        <h1 class=" text-center mb-5">Prendre un Rendez-vous</h1>
        <p class="float-left">Reference :{{ $route.params.reference }}</p>
        <div class="row float-right ">
           <router-link :to="{path: '/add/' + $route.params.reference }"> <button class="btn rendez " name="submit" >+</button></router-link> 
        </div>
    <table class="table table-striped justify-content-center">
        <thead>
          <tr>
            <th scope="col">Id</th>
            <th scope="col">Date</th>
            <th scope="col">Horaire</th>
            <th scope="col">typeconsult</th>
            <th></th>

          </tr>
        </thead>
        <tbody >
          <tr v-for="result in results" :key="result.id">
             <template v-if="editClient.id == result.id">
              <td>{{result.id}}</td>
                <td><input v-model="editClient.date" type="date"></td>
                <td><input v-model="editClient.horaire" type="text"></td>
                <td><input v-model="editClient.typeconsult" type="text"></td>
                <td><button class="btn btn-success btn-sm rounded-0" @click="update();" ><i class="material-icons" >update</i></button> &nbsp;<button class="btn btn-danger btn-sm rounded-0" @click="cancel();"><i class="material-icons" >cancel</i></button></td>
            </template>
          <template v-else>
            <td>{{result.id}}</td>
            <td>{{result.date}}</td>
            <td>{{result.horaire}}</td>
            <td>{{result.typeconsult}}</td>
            <td><button class="btn rendez" v-on:click="edit(result.id)"  ><i class="material-icons" >edit</i></button>&nbsp;
              <button class="btn rendez" v-on:click="getid(result.id);del()" name="delete"><i class="material-icons">delete</i></button></td>
              </template>
          </tr>
        </tbody>
      </table>
    
    </div>
</template>

<script>
export default {
  name:'affichage',
  data(){
    return {
      results :[],
      idR:'',
      ref:this.$route.params.reference ,
       editReser:'',
          editClient:{
              id:'',
              date:'',
              horaire:'',
              typeconsult:''
    }
    }
  },
  mounted:function(){
    this.getAll();
  },
  methods:{
    getAll(){

      fetch("http://localhost/brief6/Rendezvous/afficher/"+this.ref)
      .then(res => res.json())
      .then(data => this.results = data)
      .catch(err => console.log(err.message))
    },
    getid(x){
      this.idR=x;
      console.log(this.idR);

    },
    del(){
      fetch("http://localhost/brief6/Rendezvous/delete/"+this.idR,{
         method: "DELETE",

      })
      .then(this.getAll())

    },
       edit(x){
        fetch('http://localhost/brief6/Rendezvous/edit/'+x)
        .then(response => response.json())
        .then(data =>{
            this.editReser=data[0].id;
            this.editClient.id=data[0].id;
            this.editClient.date=data[0].date;
            this.editClient.horaire=data[0].horaire;
            this.editClient.typeconsult=data[0].typeconsult;
        })
    },
       async update(){
        console.log(JSON.stringify(this.editClient));
       await fetch('http://localhost/brief6/Rendezvous/update',{
            method: 'PUT',
            headers:{
                'Content-Type': 'application/json',
            },
           body: JSON.stringify({

                    date: this.editClient.date,
                    horaire: this.editClient.horaire,
                    typeconsult: this.editClient.typeconsult,
                    id : this.editClient.id
            })
        });
       
     await this.getAll();
            this.editReser='';
            this.editClient.id='';
            this.editClient.date='';
            this.editClient.horaire='';
            this.editClient.typeconsult='';
       },
      cancel(){
          this.editReser='';
          this.editClient.id='';
            this.editClient.date='';
            this.editClient.horaire='';
            this.editClient.typeconsult='';
    }

  
  }
};
</script>

<style>
body{
    margin: 0;
    padding: 0;
   
}
th{
    color: #407088;
}
i{
    color: white;
    cursor: pointer;
}
.rendez{
    background-color: #407088;
    color: white;
   

}
.rendez:hover{
    background-color: #4A829E;
}
h1{
    padding-top: 20px;
    color: #407088;
    
}
table{
    margin-top: 10%;
}
p{
    color: #407088;
}
</style>