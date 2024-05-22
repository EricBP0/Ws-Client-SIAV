<template>
  <div class="card card-user">
    <div class="image">
      <img style="display: block; margin-left: auto; margin-right: auto; width: 50%;" src="static/img/logo-siav.png" alt="...">
    </div>
    <div>
      <div class="author">
        <a href="#">
          <img style="display: block; margin-left: auto; margin-right: auto; width: 35%;" class="avatar border-gray" :src="$props.img" alt="...">
        </a>
      </div>
    </div>
    <div class="card-footer">
      <hr>
      <div class="button-container">
        <div class="row">
          <div class="col-lg-3 col-md-6 col-6 ml-auto">
            <h5>{{$props.name}}
              <br>
              <small>Marca</small>
            </h5>
          </div>
          <div class="col-lg-4 col-md-6 col-6 ml-auto mr-auto">
            <h5>R$ {{$props.preco}}
              <br>
              <small>Diária</small>
            </h5>
          </div>
          <div class="col-lg-3 mr-auto">
            <h5>{{$props.grupo}}
              <br>
              <small>Grupo</small>
            </h5>
          </div>
        </div>
        <slot name="button"></slot>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    props:{
      name:{
        type: String,
        description: 'Marca/Modelo do veiculo'
      },
      preco:{
        type: Number,
        description: 'Preço Diária do veiculo'
      },
      grupo:{
        type: String,
        description: 'Grupo do veiculo'
      },
      img:{
        type: String,
        description: 'foto da mota'
      }
    }
  }
</script>

<style scoped>

</style>
