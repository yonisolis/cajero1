<template>
  <div>
    <div v-if="auth === false">
      <login @enter="validar"/>
    </div>
    <div v-else>
      <cajero :usuarioActivo = usuarioValidado />
    
    </div>
  </div>
</template>

<script>
import Cajero from './components/Cajero.vue'
import Login from './components/Login.vue'
import { constants } from 'crypto';
import { async } from 'q';

export default {
  name: 'app',
  components: {
    Cajero,
    Login
  },
  data() {
    return {
      auth: false,
      usuarios: [],
      usuarioValidado:{},
      servicios:[]
    }
  },
  mounted() {
    this.cargarUsuarios()
  },
  methods: {
    cargarUsuarios: async function() {
      const data = await fetch( './usuarios.json' )
      this.usuarios = await data.json()
    },
    validar( pass ) {
      const cantidadUsuarios = this.usuarios.length
      for( let i = 0; i < cantidadUsuarios && this.auth == false ; i++) {
        
        if(this.usuarios[i].contraseña ==pass){
          this.usuarioValidado = this.usuarios[i]
          this.auth= true;   
        }
      }
    },
  
  },
 
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');
  body {
    margin: 0;

    font-family: "Source Sans Pro", sans-serif;

    background-color: #2a333d;
}
</style>
