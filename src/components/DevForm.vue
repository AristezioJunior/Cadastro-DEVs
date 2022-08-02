<template>
  <Message :msg="msg" v-show="msg" />
  <div>
    <form id="dev-form" method="POST" @submit="createDev">
      <div class="input-container">
        <label for="nome">Nome do cliente:</label>
        <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome" autocomplete="off">
      </div>
      <div class="input-container">
        <label for="desenvolvimento">Área de Atuação:</label>
        <select name="desenvolvimento" id="desenvolvimento" v-model="desenvolvimento">
          <option value="">Selecione</option>
          <option v-for="desenvolvimento in desenvolvimentos" :key="desenvolvimento.id" :value="desenvolvimento.tipo">{{ desenvolvimento.tipo }}</option>
        </select>
      </div>
      <div class="input-container">
        <label for="situacao">Situação Atual:</label>
        <select name="situacao" id="situacao" v-model="situacao">
          <option value="">Selecione </option>
          <option v-for="situacao in situacoes" :key="situacao.id" :value="situacao.tipo">{{ situacao.tipo }}</option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="idioma">Selecione os idiomas que domina:</label>
        <div class="checkbox-container" v-for="idioma in idiomadata" :key="idioma.id">
          <input type="checkbox" name="idioma" v-model="idiomas" :value="idioma.tipo">
          <span>{{ idioma.tipo }}</span>
        </div>
      </div>
      <div class="input-container">
        <input class="submit-btn" type="submit" value="Criar Cadastro!">
      </div>
    </form>
  </div>
</template>

<script>
import Message from './Message'

export default {
  name: "DevForm",
  data() {
    return {
      desenvolvimentos: null,
      situacoes: null,
      idiomadata: null,
      nome: null,
      desenvolvimento: null,
      situacao: null,
      idiomas: [],
      status: "Junior",
      msg: null
    }
  },
  methods: {
    async getDesenvolvedor() {
      const req = await fetch('http://localhost:3000/desenvolvedor')
      const data = await req.json()

      this.desenvolvimentos = data.desenvolvimento
      this.situacoes = data.situacao
      this.idiomadata = data.idioma
    },
    async createDev(e) {

      e.preventDefault()

      const data = {
        nome: this.nome,
        desenvolvimento: this.desenvolvimento,
        situacao: this.situacao,
        idiomas: Array.from(this.idiomas),
        status: "Junior"
      }

      const dataJson = JSON.stringify(data)    

      const req = await fetch("http://localhost:3000/Devs", {
        method: "POST",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      });

      const res = await req.json()

      console.log(res)

      //colocar uma msg no sistema
      this.msg = `${res.nome} Cadastro realizado com sucesso!`

      // clear message
      setTimeout(() => this.msg = "", 3000)

      // limpar campos
      this.nome = ""
      this.desenvolvimento = ""
      this.situacao = ""
      this.idioma = []
      
    }
  },
  mounted () {
    this.getDesenvolvedor()
  },
  components: {
    Message
  }
}
</script>

<style scoped>
  #dev-form {
    max-width: 400px;
    margin: 0 auto;
  }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }

  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }

  input, select {
    padding: 5px 10px;
    width: 300px;
  }

  #opcionais-container {
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