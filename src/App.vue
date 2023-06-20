<template>

  <div id="app">
    <h1>Crypto-Coins</h1>
      <div class="Header">
    
    <img class="img_logo" src="./assets/logo.png" alt="">
     </div>
    <h2>Formulario Alta de Usuarios</h2>
    <FormularioClientes @emit-form="addUser" />
    <button v-if="users.length > 0" @click="showInfoTabla = true" class="ver-info-btn">Ver información de clientes</button>
    <TablaClientes v-if="showInfoTabla" :users="users" />
  
   
  </div>
</template>

<script>
import FormularioClientes from "./components/FormularioClientes.vue";
import TablaClientes from "./components/TablaClientes.vue";

export default {
  name: "App",
  components: {
    FormularioClientes,
    TablaClientes,
  },
  data() {
    return {
      users: [],
      showInfoTabla:false,
    };
  },
  methods: {
    addUser(user) {
      this.users.push(user);
      this.showInfoTabla= true;
    },
  },
};
</script>

<style>
#app {
  font-family:'Courier New', Courier, monospace;
  font-style: oblique;
  text-align: center;
  color: #2c3e50;
  margin-top: 50px;
}
 
h1{
  background-color: aquamarine;
   padding-bottom: 10px;
   margin: 5px;
}

 
h2{
  background-color: aquamarine;
   padding-bottom: 10px;
   margin: 5px;
}

.Header{
  background-color: rgb(90, 232, 203);
  margin: 4px;
}

.img_logo {
 max-width: 200px;
 min-width: 200px;
 background-color: rgb(228, 230, 247);


}

.ver-info-btn {
  margin-top: 20px;
  background-color: #3de110;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
