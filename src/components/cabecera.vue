<template>

  <section class="cabecera">

    <nav class="navbar navbar-expand-lg navbar-light">
      <div class="navbar-brand">
        <router-link to="/"><img class="logo mr-2" src="../assets/phoenix2.svg" alt="Logo PhoneixGames">PhoenixGames</router-link>
      </div>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item authenticated">
            <a class="authenticated" v-if="authenticated">{{user.data.email}}</a>
            <router-link to="/login" v-if="!authenticated">Iniciar sesion</router-link>
          </li>
          <li class="nav-item">
            <!--  @click.native para que el evento funcione en router-link -->
            <router-link to="/" @click.native="logout" v-if="authenticated">Cerrar sesion</router-link>
            <router-link to="/login" v-if="!authenticated">Registrarse</router-link>
          </li>
          <li class="nav-item">
            <router-link to="/carrito" v-if="authenticated">Carrito de compra</router-link>
          </li>
        </ul>
      </div>
    </nav>

  </section>

</template>

<script lang="js">

import firebase from '../db.js'
  export default  {
    name: 'cabecera',
    props: [],
    mounted(){
      firebase.auth.onAuthStateChanged( user => {
        if (user) {
          this.user.loggedIn = true;
          this.user.data = user;
        }
        else {
          this.user.loggedIn = false;
          this.user.data = {};
        }
      })


    },
    data () {
      return {
        user: {
          loggedIn: false,
          data: {}
        },
      }
    },
    methods: {
      logout:function (){
        firebase.logout()
      }
    },
    computed: {
          authenticated(){
              return this.user.loggedIn
          },
      },
    components:{
    }
}


</script>

<style scoped lang="scss">
  @import "../scss/main.scss";
</style>
