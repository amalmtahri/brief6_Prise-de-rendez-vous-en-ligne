<template>
    <!-- eslint-disable -->
    <div class="container-fluid affcont">
        <div class="row">
            <div class="col-9" style="text-align:left">
            reference:{{ref}}
            </div>
            <div class="col-3 ">
            <button class="btn btn-primary" style="width:10vh">+</button>
            </div>
        </div>
        <div class="row">
            <div class="table-responsive tabcont">
                <table class="table">
                    <thead>
                        <tr>
                            <th>Date</th>
                            <th>Horaire</th>
                            <th>Type Consultation</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="elemt in rdvs" v-bind:key="elemt.id">
                            <template v-if="editClient.id == elemt.id">
                                <td><input v-model="editClient.date" type="date"></td>
                                <td><input v-model="editClient.horaire" type="text"></td>
                                <td><input v-model="editClient.typeCons" type="text"></td>
                                <td><button class="btn btn-danger btn-sm rounded-0" @click="update();" >Enregistrer</button> &nbsp;<button class="btn btn-info btn-sm rounded-0" @click="cancel();">Annuler</button></td>
                            </template>
                            <template v-else>
                                <td>{{ elemt.date }}</td> 
                                <td>{{ elemt.horaire }}</td>  
                                <td>{{ elemt.typeCons }}</td>
                                <td><button class="btn btn-danger btn-sm rounded-0" @click="slectU(elemt.id); del();" >supprimer</button> &nbsp;<button class="btn btn-info btn-sm rounded-0" @click="edit(elemt.id);">modifier</button></td>
                            </template>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
<script>
/* eslint-disable */
export default {
    name: "RendezVous",
  data(){
      return{
          rdvs:[],
          ref:'',
          curentElment:'',
          editReser:'',
          editClient:{
              id:'',
              date:'',
              horaire:'',
              typeCons:''
          }
      }
  },
  mounted:function(){
      this.ref=this.$route.params.ref;
      this.getAll();
      
  },
  methods:{
    getAll(){
    fetch('http://localhost/br6-rdv/Api/rdv/afficherRdv/'+this.ref)
    .then(response => response.json())
    .then(data =>this.rdvs=data);
    },
    slectU(x){
       this.curentElment=x;
    },

    del() {
        fetch('http://localhost/br6-rdv/Api/rdv/supprimerRdv/'+this.curentElment,{
            method:"DELETE",
        })
        .then(this.getAll())
    },
    edit(x){
        fetch('http://localhost/br6-rdv/Api/rdv/getoneRdv/'+x)
        .then(response => response.json())
        .then(data =>{
            this.editReser=data.id;
            this.editClient.id=data.id;
            this.editClient.date=data.date;
            this.editClient.horaire=data.horaire;
            this.editClient.typeCons=data.typeCons;
        })
    },
    update(){
        console.log(JSON.stringify(this.editClient));
        fetch(' http://localhost/br6-rdv/Api/rdv/modifierRdv/'+this.editClient.id,{
            method: 'PUT',
            headers:{
                'Content-Type': 'application/json',
            },
            body:JSON.stringify(this.editClient),
        })
        .then(
            this.editReser='',
            this.editClient.id='',
            this.editClient.date='',
            this.editClient.horaire='',
            this.editClient.typeCons='',
            console.log(JSON.stringify(this.editClient))

           
        )
        .then(this.getAll())
    },
    cancel(){
          this.editReser='';
          this.editClient.id=''
            this.editClient.date='';
            this.editClient.horaire='';
            this.editClient.typeCons='';
    }
  }
  , created: function(){
        this.getAll()
    }
};
</script>
<style>
        .affcont{
            position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
        }
    
    .btn-info{color:#fff;}
    </style>
