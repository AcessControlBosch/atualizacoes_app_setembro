<template>

    <div class="container">

        <Header/>

        <div class="view-center">

            <p class="text-info">A máquina em utilização. A liberação foi feita com sucesso, bom trabalho! </p>
        
        </div>

        {{this.redirecionarHome()}}

    </div>

</template>

<script>

    export default{

        name: "screen_standby",

        data(){

            return{
                dataMachine: [],
            }

        },

        methods:{

            redirecionarHome: function(){

                setInterval(() => {
                    
                    this.$axios.get(this.$store.state.BASE_URL + '/machines/' + this.$store.state.idmachine).then((response) => {

                        this.dataMachine = response.data;

                        if(this.dataMachine.status === true){
                            console.log("A máquina ainda está ligada")
                        
                        } else {
                            this.$router.push('/screen_home')
                        }

                    })

                }, 15000)

            }

        }

    }

</script>

<style lang="scss" scoped>

    @import "@/layouts/_normal_pages/Screen_Standby.scss";
    @import "@/layouts/_responsividade/responsividade_grid.scss";

</style>