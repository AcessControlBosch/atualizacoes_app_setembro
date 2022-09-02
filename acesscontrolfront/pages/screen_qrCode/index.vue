<template>
    <div class="container">
    
        <HeaderWithLogout />

        <div class="content">

            <div class="row-superior">

                <TitlePage />

            </div>

            <div class="row-inferior">
                <!-- <p>{{ error }}</p>
                <p>{{ decodedString }}</p>
                <button @click="torch=!torch">TURN ON/ OFF  FLASHLIGHT</button> -->

                <div class="content-row-inferior">
                  <p class="subtitle">Escaneie aqui o QR Code da máquina</p>
                  <div class="qrCodeCamera">
                    <qrcode-stream @init="onInit" @decode="onDecode" :torch="torch"> </qrcode-stream>
                  </div>
                </div>
                
                <p class="errorP" style="color:red;">{{ error }}  </p>
      
            </div>

        </div>
    
    </div>
    
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader'
export default {
  middleware:  'auth',
  data(){
    return{
      error:'',
      decodedString:'',
      torch: true,
      valueQrCode:[],
      responseMachine: []
    }
  },
  components:{
    QrcodeStream
  },

  methods: {

    async onInit( promise ){
      try {
      
      const { capabilities } = await promise
      console.log('-----QR CODE -----')
      console.log(this.decodedString)
      // successfully initialized
    } catch (error) {
      if (error.name === 'NotAllowedError') {
        this.error = "user denied camera access permisson"
      } else if (error.name === 'NotFoundError') {
        this.error = "no suitable camera device installed"
      } else if (error.name === 'NotSupportedError') {
        this.error = "page is not served over HTTPS (or localhost)"
      } else if (error.name === 'NotReadableError') {
        this.error = "maybe camera is already in use"
      } else if (error.name === 'OverconstrainedError') {
        this.error = "did you requested the front camera although there is none?"
      } else if (error.name === 'StreamApiNotSupportedError') {
        this.error = "browser seems to be lacking features"
      }
    } finally {
      // hide loading indicator
    }
  
    },
    async onDecode(decodedString){
      console.log('-----QR CODE onDecode-----');
      console.log('onDecode', decodedString);
      await this.$store.dispatch("setidmachine", decodedString);

      this.verifyMachine();

    },

    verifyMachine: function(){

      this.$axios.get(this.$store.state.BASE_URL + '/machines/').then((response) => {

        this.responseMachine = response.data;

        let increment = 0;

        for(increment; increment < this.responseMachine.length; increment++) {

          if(this.responseMachine[increment].id === this.$store.state.idmachine){

              if(this.responseMachine[increment].statusMaint === true){

                alert("Máquina em manutenção")
                this.$auth.logout();     


              } else {

                this.$router.push('/screen_home');

              }


          }

        }

      }).catch(error => {
        console.log(error)
      })


    }



}
}
</script> 


<style lang="scss" scoped>

   @import "@/layouts/_normal_pages/Screen_qrCode.scss";
   @import "@/layouts/_responsividade/responsividade_formularios.scss";

</style>