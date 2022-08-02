<template>
  <Message :msg="msg" v-show="msg" />
  <div id="burger-table" v-if="devss">    
    <div>
      <div id="burger-table-heading">
        
        <div class="order-id">Id:</div>
        <div>Cliente:</div>
        <div>desenvolvimento:</div>
        <div>situacao:</div>
        <div>idioma:</div>
        <div>Status:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="devs in devss" :key="devs.id">
        <div class="order-number">{{ devs.id }}</div>
        <div>{{ devs.nome }}</div>
        <div>{{ devs.desenvolvimento }}</div>
        <div>{{ devs.situacao }}</div>
        <div>
          <ul v-for="(idioma, index) in devs.idiomas" :key="index">
            <li>{{ idioma }}</li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updatedevs($event, devs.id)">
            <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="devs.status == s.tipo">
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deletedevs(devs.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h2>Não há pedidos no momento!</h2>
  </div>
</template>
<script>
import Message from './Message'

  export default {
  name: "Dashboard",
  data() {
    return {
      devss: null,
      devs_id: null,
      status: [],
      msg: null
    }
  },
  methods: {
      async getPedidos() {
        const req = await fetch('http://localhost:3000/devs')

        const data = await req.json()

        this.devss = data

        // Resgata os status de pedidos
        this.getStatus()

      },
      async getStatus() {

        const req = await fetch('http://localhost:3000/status')

        const data = await req.json()

        this.status = data

      },
      async deletedevs(id) {

        const req = await fetch(`http://localhost:3000/devs/${id}`, {
          method: "DELETE"
        });

        const res = await req.json()
        
        //colocar uma msg no sistema
        this.msg = "Pedido Removido com Sucesso."

        // clear message
        setTimeout(() => this.msg = "", 3000)

        this.getPedidos()

      },
      async updatedevs(event, id) {

        const option = event.target.value;

        const dataJson = JSON.stringify({status: option});

        const req = await fetch(`http://localhost:3000/devs/${id}`, {
          method: "PATCH",
          headers: { "Content-Type" : "application/json" },
          body: dataJson
        });

        const res = await req.json()

        //colocar uma msg no sistema
        this.msg = "Pedido Alterado com Sucesso."

        // clear message
        setTimeout(() => this.msg = "", 2000)

        console.log(res)

      }
    },
  mounted () {
    this.getPedidos()
    },
  components: {
    Message
  }
  }
</script>

<style scoped>
  #burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .delete-btn {
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
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }
  
</style>