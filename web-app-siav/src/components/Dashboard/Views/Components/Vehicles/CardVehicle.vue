
<template>
  <div class="card card-user">
    <div class="card-body">
      <div class="author">
        <a href="#">
          <h5 class="title">User</h5>
        </a>
        <p class="description">
          @user
        </p>
      </div>
    </div>
    <div class="card-footer">
      <hr>
      <div class="button-container">
        <div class="row">
          <div class="col-lg-3 col-md-6 col-6 ml-auto">
            <h5>9.7
              <br>
              <small>Nota</small>
            </h5>
          </div>
          <div class="col-lg-4 col-md-6 col-6 ml-auto mr-auto">
            <h5>2 Veículos
              <br>
              <small>Alugados</small>
            </h5>
          </div>
          <div class="col-lg-3 mr-auto">
            <h5>R$ 645,00
              <br>
              <small>Gastos</small>
            </h5>
          </div>
        </div>
      </div>
      <p-button type="info" >Alugar</p-button>
    </div>
  </div>
</template>
<script>
  export default {}
    /*props:{
      grupos: [{
        tipo: null,
        veiculos:[]
      }]
    },
    methods:{
      getGrupsVehicles(){

      },
      handleAlugar(){

      }
    }
  }*/
</script>

<style scoped>

</style>
