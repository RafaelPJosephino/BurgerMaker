<template>
    <div id="burger-table">
        <Menssage :msg="msg" v-show="msg" />
        <div>
            <div id="burger-table-heading">
                <div class="order-id"></div>
                <div>cliente</div>
                <div>pao</div>
                <div>carne</div>
                <div>opcionais</div>
                <div>acoes</div>
            </div>
            <div id="burger-table-rows">
                <div class="burger-table-row" v-for="burger in burgers" :key="burger.id" >
                    <div class="order-number">{{burger.id}}</div>
                    <div>{{burger.nome}}</div>
                    <div>{{burger.pao}}</div>
                    <div>{{burger.carne}}</div>
                    <div>
                        <ul>
                            <li v-for="(opcionais,index) in burger.opcional" :key="index" >{{opcionais}} </li>
                        </ul>
                    </div>
                    <div>
                        <select name="status" class="status" @change=" updateBurger($event, burger.id)">
                            <option v-for="status in datastatus" :key="status.id" :selected="burger.status==status.tipo" >{{status.tipo}}</option>
                        </select>
                        <button class="delete-btn" @click="deleteburger(burger.id)">Cancelar</button>
                    </div>

                </div>

            </div>

        </div>
    </div>
</template>
<script>
import Menssage from './Menssage.vue';
export default {
    
    name:"Dashboard",
     components: { Menssage },

    data(){
        return{
            burgers:null,
            burgersId:null,
            datastatus:[],
            msg:null,
        }
    },
    methods:{

        async getPedidos(){
            let req = await fetch("http://localhost:3000/burgers");
            let data = await req.json();

            this.burgers = data;

            console.log(this.burgers);


        },
        async getStatus(){
            let req = await fetch("http://localhost:3000/status");
            let data = await req.json();

            this.datastatus = data;

        },
        async deleteburger(id){
            let req = await fetch(`http://localhost:3000/burgers/${id}`,{
                method:"delete"});
            let res = await req.json();

            this.getPedidos();

            this.msg = `Pedido Nº ${id} foi deletado`
            
            setTimeout(()=> this.msg = "", 3000)

        },
        async updateBurger(event,id){

            
            let option = event.target.value;

            let dataJson = JSON.stringify({status: option});
            
            let req = await fetch(`http://localhost:3000/burgers/${id}`,{method: "PATCH",headers:{"Content-Type":"application/json"},body: dataJson});
            

            let res = req.json();
           
           this.msg = `Status do pedido Nº ${id} foi alterado!`

            setTimeout(()=> this.msg = "", 3000)
        }


    },
    mounted(){
        this.getPedidos();
        this.getStatus();
    }



}
</script>
<style scoped>

#burger-table{
    max-width: 1200px;
    margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row{
    display: flex;
    flex-wrap:wrap ;
}

#burger-table-heading{
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

#burger-table-heading div,
.burger-table-row div{
width: 19%;
}

.burger-table-row{
  width:  100%;
  padding: 12px; 
  border-bottom : 1px solid #ccc;
}
#burger-table-heading .order-id,
.burger-table-row .order-number{
    width: 5%;
}

select{
    padding: 12px 6px;
    margin-right: 12px;
}
.delete-btn{
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    border-radius:5px ;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}
.delete-btn:hover{
    background-color: transparent;
    color: #222;
}


</style>