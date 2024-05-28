<template>
  <div class="row">
    <div class="col-lg-4 col-md-5" v-show="!localModal && !showModalVehicles && !finalProcess" v-for="group in groups" :key="group.name">
      <GroupVehicle :type="group.type"
                    :name="group.name"
                    :description="group.description"
                    :range="group.range"
                    :img="group.img">
        <div class="col-md-14 mt-2" slot="button">
          <p-button @click="openModal(group.name)" type="success">Escolher Grupo</p-button>
        </div>
      </GroupVehicle>
    </div>
<!--    <div v-if="showModalVehicles && !hasVehicles">
      <h3>O grupo não possui nenhum veículo ainda</h3>
      <p-button @click="" type="success">Voltar</p-button>
    </div>-->
    <div class="col-lg-4 col-md-5" v-if="showModalVehicles" v-for="vehicle in vehicles" :key="vehicle.name">
      <card-vehicle v-if="vehicle.grupo === selectedGroup"
                    :name="vehicle.name"
                    :grupo="vehicle.grupo"
                    :preco="vehicle.preco"
                    :img="vehicle.img">
        <div class="col-md-14 mt-2" slot="button">
          <p-button @click="openModalConfirm" type="success">Escolher Veículo</p-button>
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
                    <input type="text" placeholder="Usuário padrão" disabled="" class="form-control">
                  </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-6 control-label">Email</label>
                  <div class="col-sm-12">
                    <input type="text" placeholder="teste@gmail.com" disabled="" class="form-control">
                  </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-6 control-label">Telefone</label>
                  <div class="col-sm-12">
                    <input type="text" placeholder="(62) 11111-2222" disabled="" class="form-control">
                  </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-6 control-label">CPF</label>
                  <div class="col-sm-12">
                    <input type="text" placeholder="123.456.789-00" disabled="" class="form-control">
                  </div>
                </div>
              </fieldset>

              <fieldset>
                <div class="form-group">
                  <label style="font-weight: bold" class="col-sm-6 control-label">Seguros</label>
                  <div class="col-sm-12">
                    <p-checkbox>Proteção Total Avarias</p-checkbox>
                    <p-checkbox>Proteção de Vidros Premium e Pneus</p-checkbox>
                  </div>
                  <label style="font-weight: bold" class="col-sm-6 control-label">Adicionais</label>
                  <div class="col-sm-12">
                    <p-checkbox>Condutor Adicional Ilimitado</p-checkbox>
                    <p-checkbox>Condutor Jovem</p-checkbox>
                    <p-checkbox>Limpeza Garantida</p-checkbox>
                  </div>
                </div>
              </fieldset>
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
              <div class="col-md-12">
                <p-checkbox v-model="changeDates">Alterar datas</p-checkbox>
              </div>
              <div v-if="changeDates">
                <div style="margin-top: 16px" class="col-md-12">
                  <el-date-picker v-model="datePicker" type="date" placeholder="Dia de retirada"
                                  :picker-options="pickerOptions1">
                  </el-date-picker>
                </div>
                <div style="margin-top: 10px" class="col-md-4">
                  <el-date-picker v-model="datePicker" type="date" placeholder="Dia de Devolução"
                                  :picker-options="pickerOptions1">
                  </el-date-picker>
                </div>
              </div>
              <div style="margin-top: 10px" class="col-md-12">
                <p>Valor total do aluguel:</p>
                <p style="color: #4CD964; font-weight: bold"> R$ 567,89</p>
              </div>
              <div class="col-md-14 mt-2" slot="button">
                <p-button @click="finalizeRental" type="success">Finalizar aluguel</p-button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-show="finalProcess">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h4 class="card-title">Resumo do Aluguel</h4>
          </div>
          <div class="card-body">
            <form method="get" action="/" class="form-horizontal">
              <div style="margin-top: 10px" class="col-md-12">
                <p>Valor total do aluguel:</p>
                <p style="color: #4CD964; font-weight: bold"> R$ 567,89</p>
              </div>
              <div style="margin-top: 10px" class="col-md-12">
                <p>Dias alugados</p>
                <p style="font-weight: bold"> 14</p>
              </div>
              <div style="margin-top: 10px" class="col-md-12">
                <p>Veículo alugado</p>
                <p style="font-weight: bold"> Hornet</p>
              </div>
              <div class="col-md-14 mt-2" slot="button">
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
import { DatePicker } from "element-ui";
import swal from "sweetalert2";

export default {
  components: {
    [DatePicker.name]: DatePicker,
    GroupVehicle,
    CardVehicle,
  },
  props: {
    modal: Boolean,
    showModalVehicles: Boolean,
    finalProcess: Boolean,
    changeDates: Boolean,
    datePicker: String,
    hasVehicles: Boolean,
    pickerOptions1: {
      shortcuts: [{
        text: 'Hoje',
        onClick (picker) {
          picker.$emit('pick', new Date())
        }
      },
        {
          text: 'Ontem',
          onClick (picker) {
            const date = new Date()
            date.setTime(date.getTime() - 3600 * 1000 * 24)
            picker.$emit('pick', date)
          }
        },
        {
          text: 'Uma semana atrás',
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
      vehicles: [
        {
          name:"carro 1",
          preco: 134.98,
          grupo: "A",
          img: "static/img/carros/carro1.jpg"
        },
        {
          name:"carro 2",
          preco: 136.98,
          grupo: "A",
          img: "static/img/carros/carro2.jpg"
        },
        {
          name:"carro 3",
          preco: 137.98,
          grupo: "A",
          img: "static/img/carros/carro3.jpg"
        },
        {
          name:"carro 4",
          preco: 138.98,
          grupo: "A",
          img: "static/img/carros/carro4.jpg"
        },
      ],
      groups: [
        {
          type: "Carros",
          name: "A",
          description: "Carros de pequeno porte",
          range: "Media de preço 145,86",
          img: "static/img/carros/carro1.jpg"
        },
        {
          type: "Carros",
          name: "B",
          description: "Carros de grande porte",
          range: "Media de preço 165,96",
          img: "static/img/carros/carro2.jpg"
        },
        {
          type: "Carros",
          name: "C",
          description: "Carros de medio porte",
          range: "Media de preço 125,63",
          img: "static/img/carros/carro3.jpg"
        },
        {
          type: "Carros",
          name: "D",
          description: "Carros de pequeno porte mais caras",
          range: "Media de preço 205,04",
          img: "static/img/carros/carro4.jpg"
        },
        {
          type: "Carros",
          name: "E",
          description: "Carros de grande porte mais caras",
          range: "Media de preço 248,79",
          img: "static/img/carros/carro5.jpg"
        },
        {
          type: "Carros",
          name: "F",
          description: "Carros de medio porte mais caras",
          range: "Media de preço 228,07",
          img: "static/img/carros/carro6.jpg"
        },
      ],
    };
  },
  watch: {
    modal(newVal) {
      this.localModal = newVal;
    },
  },
  methods: {
    checkGroup(name){
      this.hasVehicles = false
      for (let i = 0 ; i<= this.vehicles.length; i++ ){
        console.log(this.vehicles[i])
        if(this.vehicles[i].grupo === name){
          this.hasVehicles = true
        }
      }
    },
    openModal(name) {
      /*this.checkGroup(name)*/
      this.selectedGroup = name;
      this.showModalVehicles = true;
    },
    openModalConfirm() {
      this.localModal = true;
      this.showModalVehicles = false
      this.$emit('update:modal', true);
    },
    finalizeRental() {
      this.localModal = false;
      this.finalProcess = true;
      this.$emit('update:modal', false);
    },
    success(){
      swal({
        title: "Aluguel Finalizado",
        type: "success",
        text: "Próximo passo será retirar o veículo na concessionária selecionada. Seu pedido e sua nota estão na aba de 'Alugueis'",
      }).then(()=>{
        window.location.reload()
      },(dismiss) => {
        window.location.reload()
      })
    }
  },
};
</script>

<style scoped>

</style>
