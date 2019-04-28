<template>
    
<div id = "app" class="container">
  <h3 class='text-center margin-10'>
    {{ title }}
  </h3>
  <div class="row">
    <div class="offset-md-2 col-md-8">
        <div class="card margin-10">
            <div class="card-header">
              Male:
            </div>
            <div class="card-body">
              <ul class="list-group list-group-flush">
                <li v-for="val in male" v-bind:key="val" class="list-group-item d-flex justify-content-between align-items-center" >
                    
                    {{ val }}
                    <img src="../assets/cat.png" class="img-circle"  width="30" height="30"> 
                </li>
              </ul>
            </div>
          </div>
    </div>
    <div class="offset-md-2 col-md-8">
        <div class="card margin-10">
            <div class="card-header">
              Female:
            </div>
            <div class="card-body">
              <ul class="list-group list-group-flush">
                <li v-for="val in female" v-bind:key="val" class="list-group-item d-flex justify-content-between align-items-center">
                    {{ val }}  
                  <img src="../assets/cat.png" class="img-circle"  width="30" height="30"> 
                </li>
              </ul>
            </div>
          </div>
    </div>
  </div>
</div>


</template>

<script>
import axios from 'axios'

export default {
  
  
  data () {
    return{
      title: `Cats grouped by owner's gender`,
      catUrl: '../assets/cat.png',
      owners: [{"name":"Bob","gender":"Male","age":23,"pets":[{"name":"Garfield","type":"Cat"},{"name":"Fido","type":"Dog"}]},{"name":"Jennifer","gender":"Female","age":18,"pets":[{"name":"Garfield","type":"Cat"}]},{"name":"Steve","gender":"Male","age":45,"pets":null},{"name":"Fred","gender":"Male","age":40,"pets":[{"name":"Tom","type":"Cat"},{"name":"Max","type":"Cat"},{"name":"Sam","type":"Dog"},{"name":"Jim","type":"Cat"}]},{"name":"Samantha","gender":"Female","age":40,"pets":[{"name":"Tabby","type":"Cat"}]},{"name":"Alice","gender":"Female","age":64,"pets":[{"name":"Simba","type":"Cat"},{"name":"Nemo","type":"Fish"}]}],
      male: [],
      female: []   
    }
  },
  methods:{
    // The getPetsData function process the object based on petType arrgument and assigns data into given arrays.
    getPetsData(ownersObj, petType){
      let me = this;
      ownersObj.map(function(owner) {
        if(owner.pets !== null){
          owner.pets.map(function(pet){
              if(pet.type === petType){
                (owner.gender === "Male") ? me.male.push(pet.name) : me.female.push(pet.name);
              }
          });
        }
      });
      me.male.sort();
      me.female.sort();
    }
  },
  // The created hook allows you to add code which is run if the Vue instance is created. 
  created(){
    //  Promise based HTTP client for the browser and node.js
     axios.get(`http://5c92dbfae7b1a00014078e61.mockapi.io/owners`)
      .then(res => { 
        this.getPetsData(res.data, 'Cat');
        },
        error => {
          alert("unexpected error");
          console.log(error);
        });
      
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.margin-10{
    margin: 10px 0;
}
</style>
