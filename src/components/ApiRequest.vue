<template>
  <div class="api-request">
    <form action="" class="form-horizontal">
      <div class="form-group center">
        <label for="" class="control-label col-sm-2">Numero</label>
        <div class="col-sm-12">
          <input
            type="number"
            class="form-control"
            name="numero"
            id="numero"
            v-model="form.numero"
          />
        </div>
      </div>
    </form>
    <div class="form-group">
      <button type="button" class="btn btn-primary" v-on:click="guardar()">
        Guardar
      </button>
    </div>
    
     <form action="" class="form-horizontal">
      <div class="form-group left">
        <label for="" class="control-label col-sm-2">ID</label>
        <div class="col-sm-12">
          <input
            type="number"
            class="form-control"
            name="id"
            id="id"
            v-model="id"
          />
        </div>
      </div>
    </form>
    <div class="form-group">
      <button type="button" class="btn btn-primary" v-on:click="consultar()">
        Consultar
      </button>
    </div>
    <table class="table table-hover table-dark">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Numero</th>
          <th scope="col">Tipo</th>
        </tr>
      </thead>
      
      <tbody>
        <tr >
          <th scope="row">{{ numeros!=null?numeros.id:0}}</th>
          <td>{{numeros!=null?numeros.numero:0 }}</td>
          <td>{{ numeros!=null?numeros.tipo:0}}</td>
        </tr>
      </tbody>
    </table>

    <hr>
    <hr>
    <table class="table table-hover table-dark  ">
      <thead>
        <tr>
          <th scope="col">ID</th>
          <th scope="col">Numero</th>
          <th scope="col">Tipo</th>
        </tr>
      </thead>
      
      <tbody>
        <tr v-for="lista in ListaNumeros" :key="lista.id">
          <th scope="row">{{ lista.id }}</th>
          <td>{{ lista.numero }}</td>
          <td>{{ lista.tipo }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "ApiRequest",
  data: function () {
    return {
      ListaNumeros: null,
      numeros:null,
      num:null,
      id:null,
      form: {
        numero: "",
        tipo: "",
      },
    };
  },
  methods: {
    guardar() {
      let array = [];
      for (let i of this.ListaNumeros) {
        array.push(i.numero);
      }

      if (array.includes(parseInt(this.form.numero)) == false) {
        if(parseInt(this.form.numero)%2==0){
          this.form.tipo="par";
      }
      else{
          this.form.tipo="impar";
      }
        axios
          .post(" https://adrianbolanosapi.herokuapp.com/api/v1/aleatorios", this.form)
          .then((response) => console.log(response));
        //location. reload();
        //console.log(parseInt(this.form.numero),"el dato no esta repetido")

      } else {
          alert("El dato esta repetido");
        console.log("El dato esta repetido");
      }
      
    },

  async consultar(){
       
      let response= await axios.get(" https://adrianbolanosapi.herokuapp.com/api/v1/aleatorios/"+this.id)
      this.result= response.data.data.numero;
      this.numeros= response.data.data;
      console.log(this.result);
      

      console.log(this.form.tipo);
      console.log(this.form.numero);
      console.log(this.id);
      //axios
      //.put(" https://adrianbolanosapi.herokuapp.com/api/v1/aleatorios/"+this.id,{tipo:this.form.tipo})
      //.then((response) => (console.log(response.data.data)));
      //location. reload();
      
      
    }
  },
  created() {
    axios
      .get(" https://adrianbolanosapi.herokuapp.com/api/v1/aleatorios")
      .then((response) => (this.ListaNumeros = response.data.data));
  },
};
</script>