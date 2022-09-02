<template>

    <div class="container">

      <Header />

      <div class="content">

        <div class="row-superior">

            <TitlePage />

        </div>

        <div class="row-inferior">

            <p class="subtitle">VOCÊ JÁ SABE UTILIZAR ESTA INTERFACE?</p>

            <div class="align-items-center">
            
              <button class="btn btn-sucess" v-on:click="$router.push('/screen_qrCode')">SIM</button>
              <button class="btn btn-alert" v-on:click="$router.push('/screen_tutorial')">NÃO</button>
            
            </div>

        </div>
      
      </div>
    
    </div>

</template>


<script>

export default {

  middleware: 'auth',
  name: 'screen_utilizacao',

  beforeCreate() {

    console.log(this.$store.state.admin);

    if(this.$store.state.admin === true){

      this.$router.push('/screen_admin');

    } else {

    this.$axios.get(this.$store.state.BASE_URL + '/users/').then((response) =>{
      
      this.responseData = response.data;

      let increment = 0;
      
      for(increment; increment < this.responseData.length; increment++) {

        if(this.responseData[increment].id == this.$store.state.usuario.id){

          if(this.responseData[increment].skill == false){
            
            this.$auth.logout()
            alert("Não possui habilidade")
            this.$store.dispatch("SET_USER", {});
            break

          } else {

            alert("Possui habilidade")

          }

          break

        }

      }

    }).catch((error)=>{
      console.log(error)
    })

    }

  },

  data(){

    return{

     responseData: [],
     id: 0

    }

  }

}

</script>

<style lang="scss" scoped>

    @import "@/layouts/_normal_pages/Screen_Utilizacao.scss";
    @import "@/layouts/_responsividade/responsividade_grid.scss";

</style>