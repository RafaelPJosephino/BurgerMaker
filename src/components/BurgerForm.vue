<template>
    <div>
    <Menssage :msg="msg" v-show="msg" />
    <div>
        <form id="burger-form"  @submit="createBurger">
            <div class="input-container">
                <label for="Nome:">Nome Cliente:</label>
                <input type="text" name="nome" id="nome" v-model="nome" placeholder="Digite seu nome">
            </div>
            <div class="input-container">
                <label for="pao:">Escolha o Pao:</label>
                <select name="pao" id="pao" v-model="pao">
                    <option value="">Selecione seu Pao</option>
                    <option v-for="pao in datapaes" :key="pao.id" :value="pao.tipo">{{pao.tipo}}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="carne:">Escolha a Carne:</label>
                <select name="carne" id="carne" v-model="carne">
                    <option value="" selected >Selecione o Tipo de Carne</option>
                    <option v-for="carne in datacarnes" :key="carne.id" :value="carne.tipo" :selected="carne.id==1"  >{{carne.tipo}}</option>
                   
                </select>
            </div>
            <div id="opcionais-container" class="input-container">
                <label id="opcionais-title" for="opcional:">Selecione os Opcionais:</label>
                <div class="checkbox-container" v-for="(opcional) in dataopcionais" :key="opcional.id"> 
                    <input type="checkbox" name="opcionais" v-model="Sopcional" :value="opcional.tipo">
                    <span>{{opcional.tipo}}</span>
                </div>
            </div>
            <div class="input-container">
                <input type="submit" class="submit-btn" value="Criar meu Burger!">
            </div>
        </form>
    </div>
    </div>
    

</template>
<script>
import Menssage from './Menssage.vue';
export default {
  components: { Menssage },

    name:"BurgerForm",
    data (){
        return{
            datapaes:null,
            datacarnes:null,
            dataopcionais:null,
            nome:null,
            pao:null,
            carne:null,
            
            Sopcional:[],
            msg:null,
        }
    },

    methods:{
        async getIngredientes(){
            let req = await fetch("http://localhost:3000/ingredientes");
            let data = await req.json();

            this.datapaes = data.paes;
            this.datacarnes = data.carnes;
            this.dataopcionais = data.opcionais;
            

        },
        async createBurger(e){
            e.preventDefault();
            let data = {

                nome:this.nome,
                pao:this.pao,
                carne:this.carne,
                opcional: Array.from( this.Sopcional ),
                status: "Solicitado",                

            }
            
            let  datajson = JSON.stringify(data);

            let req = await fetch("http://localhost:3000/burgers",{
                method:"POST",
                headers:{"Content-Type":"application/json"},
                body: datajson
            });

            let res = await req.json();

            this.msg = `Pedido Nº ${res.id} realizado com susesso`
            
            
            this.nome = "";
            this.pao ="";
            this.carne  ="";
            this.Sopcional=[];
   
   

            setTimeout(()=> this.msg = "", 3000)
            

        },
        

    },
    mounted(){
        this.getIngredientes() 
    this.pao ="";
            this.carne  ="";
    
        
    },
}
</script>
<style scoped>

#burger-form{
    width: 400px;
    margin: 0 auto;
    
    
}
.input-container{
    display: flex;
    flex-direction: column; 
    margin-bottom:20px ;

}
label{
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
}
input,select{
    padding: 5px 10px;
    width: 100%;

}
#opcionais-container{
    flex-direction: row;
    flex-wrap: wrap;

}
#opcionais-title {
    width: 100%;
  }
  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }
  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }
  .submit-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }




</style>