<template>
  <div class="card">
    <div class="card-header">
      <h4 class="title">{{title}}</h4>
    </div>
    <div class="card-body">
      <ul class="list-unstyled team-members">
        <li>
          <div class="row" v-for="member in members">
            <div class="col-md-7 col-7">
              {{member.name}}
              <br>
              <span :class="getStatusClass(member.status)">
                <small>{{member.status}}</small>
              </span>
            </div>

            <div class="col-md-3 col-3 text-right">
              <p-button size="sm" type="success" outline round @click="openModalDescriptionRentedVehicles(member)">
                <i>Descrição</i>
              </p-button>
            </div>
          </div>
          <modal>
            <h6>Descrição aluguel veiculo</h6>
            <p><i class="fas fa-car"></i> Carro alugado: {{this.vehicles.name}}</p>
            <p><i class="fas fa-circle-information"></i>Status: {{this.vehicles.status}}</p>
          </modal>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
  import {Button} from 'src/components/UIComponents'
  import Modal from "@/components/UIComponents/Modal.vue";
  export default {
    components: {
      Modal,
      [Button.name]: Button
    },
    data () {
      return {
        title: 'Veículos Alugados',
        members: [
          {
            name: 'HB20',
            status: 'Offline'
          },
          {
            name: 'Etius Cros',
            status: 'Online'
          },
        ],
        vehicles: {
          name: null,
          status: null
        },
        showModalVehicles: false
      }
    },
    methods: {
      getStatusClass (status) {
        switch (status) {
          case 'Offline':
            return 'text-muted'
          case 'Available':
            return 'text-success'
          case 'Busy':
            return 'text-danger'
          default:
            return 'text-success'
        }
      },
      openModalDescriptionRentedVehicles(member){
        this.vehicles.name = member.name
        this.vehicles.status = member.status
        this.showModalVehicles = true
      }
    }
  }

</script>
<style>

</style>
