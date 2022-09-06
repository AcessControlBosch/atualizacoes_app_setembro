<template>

    <div class="container">

        <HeaderWithLogout />

        <div class="row-superior">

            <TitleRegister />

        </div>

        <div class="row-inferior">
        
        <div v-for="(register, id) in registerMachine.reverse()" :key="id" class="content-card">

            <div class="card">
            
                <div class="title-card">
                    <p>{{register.idAssociateFK.name}}</p>
                </div>

                <div class="title-info-card">
                    <p>Dados do Colaborador:</p>
                </div>

                <div class="information-card">
                    <p>Nome: {{register.idAssociateFK.name}}</p>
                    <p>EDV: {{register.idAssociateFK.EDV}}</p>
                </div>

                <div class="title-info-card">
                    <p>Dados da utilização:</p>
                </div>

                <div class="information-card">
                    <p>Horário Inicial: {{register.hour}}</p>
                    <p>Horário Final: {{register.hourFinish}}</p>
                    <p>Data de utilização: {{register.date}}</p>
                </div>
            
            </div>
        
        </div>

        </div>

    </div>

</template>

<script>

export default{

    name: "screen_registros",

    mounted() {

        this.$axios.get(this.$store.state.BASE_URL + "/releasemachines/").then((response) => {

            this.responseRegisters = response.data;

            let increment = 0;

            for(increment; increment < this.responseRegisters.length; increment++) {


               if(this.responseRegisters[increment].idMachineFK.id.toString() === this.$store.state.idmachine){

                    this.registerMachine.push(this.responseRegisters[increment]);

               }
               
            }

        }).catch((error) => {

            console.log(error)

        });
        
    },

    data(){

        return{
            responseRegisters: [],
            registerMachine:[]
        }

    }

}

</script>

<style lang="scss" scoped>

    @import "@/layouts/_responsividade/responsividade_grid.scss";
    @import "@/layouts/_normal_pages/Screen_Registros.scss";

</style>