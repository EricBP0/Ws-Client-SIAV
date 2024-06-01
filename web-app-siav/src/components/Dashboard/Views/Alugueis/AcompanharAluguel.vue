<template>
  <div>
    <card v-loading="loading">
      <h5 slot="header" class="card-title">Alugueis</h5>
      <collapse v-if="alugueis && alugueis.length > 0">
        <collapse-item v-for="aluguel in alugueis" :title="aluguel.nome" :name="aluguel.id" :key="aluguel.id">
          <div class="row">
            <h6>Id do Registro:</h6>
            <p style="padding-left: 5px">{{ aluguel.id }}</p>
          </div>
          <div class="row">
            <h6>Nome no registro:</h6>
            <p style="padding-left: 5px">{{ aluguel.nome }}</p>
          </div>
          <div class="row">
            <h6>Data de Retirada:</h6>
            <p style="padding-left: 5px">{{ aluguel.dataInicio }}</p>
          </div>
          <div class="row">
            <h6>Data de Retorno:</h6>
            <p style="padding-left: 5px">{{ aluguel.dataFim }}</p>
          </div>
          <div class="row">
            <h6>Seguros:</h6>
          </div>
          <div class="row" v-for="seguro in aluguel.seguros" :key="seguro.id">
            <p style="padding-left: 5px">{{ seguro.descricao }}</p>
            <p style="padding-left: 5px">R$ {{ seguro.valor }}</p>
          </div>
          <div class="row">
            <h6>Adicionais</h6>
          </div>
          <div class="row" v-for="adicional in aluguel.adicionais" :key="adicional.id">
            <p style="padding-left: 5px">{{ adicional.descricao }}</p>
            <p style="padding-left: 5px">R$ {{ adicional.valor }}</p>
          </div>
          <div class="row">
            <h6>Veiculo:</h6>
            <p style="padding-left: 5px">{{ aluguel.veiculo.marca }}</p>
            <p style="padding-left: 5px">{{ aluguel.veiculo.grupoId.nome }}</p>
          </div>
          <div class="row">
            <h6>Diária:</h6>
            <p style="padding-left: 5px">R$ {{ aluguel.veiculo.diaria }}</p>
          </div>
          <div class="row">
            <h6>Valor final do aluguel:</h6>
            <p style="color: #4CD964; font-weight: bold; padding-left: 5px">R$ {{ aluguel.valor }}</p>
          </div>
        </collapse-item>
      </collapse>
      <div v-else>
        <p>Não há aluguéis disponíveis para este cliente.</p>
      </div>
    </card>
  </div>
</template>

<script>
import { Collapse, CollapseItem } from "@/components/UIComponents";

export default {
  components: {
    Collapse,
    CollapseItem,
  },
  data() {
    return {
      cliente: null,
      alugueis: null,
      loading: false,
    };
  },
  mounted() {
    this.getCliente();
    this.getAlugueisByCliente();
  },
  methods: {
    getCliente() {
      const cliente = localStorage.getItem('cliente');
      if (cliente !== null) {
        this.cliente = JSON.parse(cliente);
      } else {
        this.$router.push('/login');
      }
    },
    async getAlugueisByCliente() {
      const cliente = this.cliente;
      this.loading = true
      if (cliente !== null) {
        try {
          const response = await fetch(`http://localhost:8081/aluguel/${cliente.id}`, {
            method: "GET",
            headers: {
              "Content-Type": "application/json"
            },
          });
          const data = await response.json();
          this.alugueis = data;
          console.log(data)
          console.log(this.alugueis)
          this.loading = false
        } catch (error) {
          console.error(error);
          this.loading = false
        }finally {
          this.loading = false;
        }
      }else{
        this.loading = false
      }
    },
  },
};
</script>
<style scoped>

</style>
