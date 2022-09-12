<template>

    <div class="container">

        <HeaderWithPathHome />

        <div class="row-superior">

            <TitleRegister />

        </div>

        <div class="row-inferior">

            <div class="content-table">

                <div class="index-table">

                    <table class="table-register">

                        <tr class="tr-register">

                            <th class="th-register">Name</th>
                            <th class="th-register">Date</th>
                            <th class="th-register">Hour</th>

                        </tr>

                        <tr v-for="(register, id) in registerMachine.reverse()" :key="id" class="tr-register">
                           <td class="td-register">{{register.idAssociateFK.name}}</td> 
                           <td class="td-register">{{register.date}}</td>
                           <td class="td-register">{{register.hour}}</td>  
                        </tr>


                    </table>
                    
                </div>

            </div>


        
        <!-- <div v-for="(register, id) in registerMachine.reverse()" :key="id" class="content-card"> -->
        
        <!-- </div> -->

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