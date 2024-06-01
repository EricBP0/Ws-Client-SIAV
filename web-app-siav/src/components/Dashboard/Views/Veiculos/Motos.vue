<template>
  <div class="row">
    <div class="col-lg-4 col-md-5" v-show="!localModal && !showModalVehicles && !finalProcess" v-for="group in groups" :key="group.name">
      <GroupVehicle :type="group.type"
                    :name="group.nome"
                    :description="group.descricao"
                    :range="group.media">
        <div class="col-md-14 mt-2" slot="button">
          <p-button @click="openModal(group)" type="success">Escolher Grupo</p-button>
        </div>
      </GroupVehicle>
    </div>
    <div class="col-lg-4 col-md-5" v-if="showModalVehicles" v-for="vehicle in vehicles" :key="vehicle.name">
      <card-vehicle v-if="vehicle.grupo === selectedGroup.id"
                    :name="vehicle.marca"
                    :grupo="selectedGroup.nome"
                    :preco="vehicle.diaria">
        <div class="col-md-14 mt-2" slot="button">
          <p-button @click="selectVehicle(vehicle)" type="success">Escolher Veículo</p-button>
        </div>
      </card-vehicle>
    </div>
    <div class="row">
      <div class="col-md-12 ml-auto mr-auto" v-show="localModal && !showModalVehicles">
        <div class="card">
          <div class="card-header">
            <h4 class="card-title">Finalizar Registro de Aluguel</h4>
          </div>
          <div class="card-body">
            <form method="get" action="/" class="form-horizontal">
              <fieldset>
                <div class="form-group">
                  <label class="col-sm-12 control-label">Nome</label>
                  <div class="col-sm-12">
                    <input type="text" :placeholder="cliente.clienteNome" disabled class="form-control">
                  </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-6 control-label">Email</label>
                  <div class="col-sm-12">
                    <input type="text" :placeholder="cliente.email" disabled class="form-control">
                  </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-6 control-label">Telefone</label>
                  <div class="col-sm-12">
                    <input type="text" :placeholder="cliente.telefone" disabled class="form-control">
                  </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-6 control-label">CPF</label>
                  <div class="col-sm-12">
                    <input type="text" :placeholder="cliente.cpfCnpj" disabled class="form-control">
                  </div>
                </div>
              </fieldset>

                <div>
                  <label style="font-weight: bold" class="col-sm-6 control-label">Seguros</label>
                  <div class="col-sm-12">
                    <fg-input>
                      <el-select multiple class="select-primary"
                                 size="large"
                                 v-model="selectedSeguros"
                                 placeholder="Seguros">
                        <el-option v-for="seguro in seguros"
                                   class="select-primary"
                                    :key="seguro.id"
                                    :value="seguro.valor"
                                    :label="seguro.descricao">
                        </el-option>
                      </el-select>
                    </fg-input>
                  </div>
                  <label style="font-weight: bold" class="col-sm-6 control-label">Adicionais</label>
                  <div class="col-sm-12">
                    <fg-input>
                      <el-select multiple class="select-primary"
                                 size="large"
                                 v-model="selectedAdicionais"
                                 placeholder="Adicionais">
                        <el-option v-for="adicional in adicionais"
                                   class="select-primary"
                                   :key="adicional.id"
                                   :value="adicional.valor"
                                   :label="adicional.descricao">
                        </el-option>
                      </el-select>
                    </fg-input>
                  </div>
                </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <div style="margin-left: 24px" class="row">
      <div class="col-md-12" v-show="localModal && !showModalVehicles">
        <div class="card">
          <div class="card-header">
            <h4 class="card-title">Resumo do Aluguel</h4>
          </div>
          <div class="card-body">
            <form method="get" action="/" class="form-horizontal">
              <div>
                <div style="margin-top: 16px" class="col-md-12">
                  <el-date-picker v-model="dateInicio" type="date" placeholder="Dia de retirada"
                                  :picker-options="pickerOptions1">
                  </el-date-picker>
                </div>
                <div style="margin-top: 10px" class="col-md-4">
                  <el-date-picker v-model="dateFim" type="date" placeholder="Dia de Devolução"
                                  :picker-options="pickerOptions1">
                  </el-date-picker>
                </div>
              </div>
              <div style="margin-top: 10px" class="col-md-12">
                <p>Valor total do aluguel:</p>
                <p style="color: #4CD964; font-weight: bold">{{ totalAluguel | currency }}</p>
              </div>
              <div class="col-md-14 mt-2">
                <p-button @click="finalizeRental" type="success">Finalizar aluguel</p-button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="finalProcess">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h4 class="card-title">Resumo do Aluguel</h4>
          </div>
          <div class="card-body">
            <form method="get" action="/" class="form-horizontal">
              <div style="margin-top: 10px" class="col-md-12">
                <p>Valor total do aluguel:</p>
                <p style="color: #4CD964; font-weight: bold">{{ totalAluguel | currency }}</p>
              </div>
              <div style="margin-top: 10px" class="col-md-12">
                <p>Dias alugados</p>
                <p style="font-weight: bold">{{ diasAlugados }}</p>
              </div>
              <div style="margin-top: 10px" class="col-md-12">
                <p>Veículo alugado</p>
                <p style="font-weight: bold">{{ selectedVehicle.marca }}</p>
              </div>
              <div class="col-md-14 mt-2">
                <p-button @click="success" type="success">Finalizar aluguel</p-button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardVehicle from "@/components/Dashboard/Views/Components/Vehicles/CardVehicle.vue";
import GroupVehicle from "@/components/Dashboard/Views/Components/Vehicles/GroupVehicle.vue";
import { DatePicker, Select, Option } from "element-ui";
import swal from "sweetalert2";

export default {
  components: {
    [DatePicker.name]: DatePicker,
    [Select.name]: Select,
    [Option.name]: Option,
    GroupVehicle,
    CardVehicle,
  },
  props: {
    modal: Boolean,
    showModalVehicles: Boolean,
    finalProcess: Boolean,
    changeDates: Boolean,
    dateInicio: String,
    dateFim: String,
    pickerOptions1: {
      shortcuts: [{
        text: 'Today',
        onClick (picker) {
          picker.$emit('pick', new Date())
        }
      },
        {
          text: 'Yesterday',
          onClick (picker) {
            const date = new Date()
            date.setTime(date.getTime() - 3600 * 1000 * 24)
            picker.$emit('pick', date)
          }
        },
        {
          text: 'A week ago',
          onClick (picker) {
            const date = new Date()
            date.setTime(date.getTime() - 3600 * 1000 * 24 * 7)
            picker.$emit('pick', date)
          }
        }]
    },
  },
  data() {
    return {
      localModal: this.modal,
      selectedGroup: '',
      selectedVehicle: null,
      nomeAluguel: null,
      aluguel: {},
      vehicles: [],
      groups: [],
      seguros: [],
      selectedSeguros: [],
      selectedAdicionais: [],
      adicionais: [],
      cliente: null,
      totalAluguel: 0,
      diasAlugados: 0
    };
  },
  watch: {
    modal(newVal) {
      this.localModal = newVal;
    },
    selectedSeguros() {
      this.calculateTotal();
    },
    selectedAdicionais() {
      this.calculateTotal();
    },
    dateInicio() {
      this.calculateTotal();
    },
    dateFim() {
      this.calculateTotal();
    }
  },
  mounted() {
    this.getVehicles();
    this.getGroups();
    this.getCliente();
    this.getAdicionaisSeguros();
  },
  methods: {
    openModal(group) {
      this.selectedGroup = group;
      this.showModalVehicles = true;
      this.$emit('update:modal', true);
    },
    selectVehicle(vehicle) {
      this.selectedVehicle = vehicle;
      this.openModalConfirm();
    },
    openModalConfirm() {
      this.getCliente();
      if (this.cliente !== null) {
        this.localModal = true;
        this.showModalVehicles = false;
        this.$emit('update:modal', true);
        this.calculateTotal();
      }
    },
    finalizeRental() {
      this.localModal = false;
      this.finalProcess = true;
      this.$emit('update:modal', false);
    },
    async success() {
      this.aluguel = {
        nome: this.selectedVehicle.nome + this.selectedVehicle.placa,
        veiculo: this.selectedVehicle,
        dataInicio: this.dateInicio,
        dataFim: this.dateFim,
        valor: this.totalAluguel,
        cliente: this.cliente,
        adicionais: this.adicionais,
        seguros: this.seguros
      }

      try {
        const response = await fetch('http://localhost:8081/aluguel/new', {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.aluguel)
        });
        swal({
          title: "Aluguel Finalizado",
          type: "success",
          text: "Próximo passo será retirar o veículo na concessionária selecionada. Seu pedido e sua nota estão na aba de 'Alugueis'",
        }).then(() => {
          this.$route.path("/alugueis/acompanhar-aluguel");
        }, (dismiss) => {
          this.$route.path("/alugueis/acompanhar-aluguel");
        });
      } catch (error) {
        console.error(error);
      }

    },
    async getVehicles() {
      try {
        const response = await fetch('http://localhost:8081/veiculos/motos', {
          method: "GET",
          headers: {
            "Content-Type": "application/json"
          },
        });
        const data = await response.json();
        this.vehicles = data;
      } catch (error) {
        console.error(error);
      }
    },
    async getAdicionaisSeguros() {
      try {
        const response = await fetch('http://localhost:8081/aluguel/adicionais-seguros', {
          method: "GET",
          headers: {
            "Content-Type": "application/json"
          },
        });
        const data = await response.json();
        this.seguros = data.seguros;
        this.adicionais = data.adicionais;

      } catch (error) {
        console.error(error);
      }
    },
    async getGroups() {
      try {
        const response = await fetch('http://localhost:8081/grupo/motos', {
          method: "GET",
          headers: {
            "Content-Type": "application/json"
          },
        });
        const data = await response.json();
        this.groups = data;
      } catch (error) {
        console.error(error);
      }
    },
    getCliente() {
      const cliente = localStorage.getItem('cliente');
      if (cliente !== null) {
        this.cliente = JSON.parse(cliente);
      } else {
        this.$router.push('/login');
      }
    },
    calculateTotal() {
      if (!this.dateInicio || !this.dateFim || !this.selectedVehicle) {
        this.totalAluguel = 0;
        return;
      }

      const date1 = new Date(this.dateInicio);
      const date2 = new Date(this.dateFim);
      this.diasAlugados = (date2 - date1) / (1000 * 60 * 60 * 24);

      let total = this.selectedVehicle.diaria * this.diasAlugados;


      this.selectedAdicionais.forEach(adicional => {
        total += adicional;
      });

      this.selectedSeguros.forEach(seguro => {
        total += seguro;
      });


      this.totalAluguel = total;
    }
  },
  filters: {
    currency(value) {
      return `R$ ${parseFloat(value).toFixed(2)}`;
    }
  }
};
</script>

<style scoped>

</style>
