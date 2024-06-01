<template>
  <div class="card card-user">
    <div class="card-avatar">
      <p style="text-align: center; margin-top: 15px" class="description">
        {{$props.description}}
      </p>
      <p style="text-align: center; margin-top: 6px; font-weight: bold; color: #4CD964" class="description">
        Media de R$ {{ `${$props.range}` }}
      </p>
    </div>
    <div class="card-footer">
      <hr>
      <div class="button-container">
        <div class="row">
          <div class="col-lg-3 col-md-14 ml-auto mr-auto">
            <h5>{{$props.name}}
            </h5>
          </div>
        </div>
        <slot name="button"> </slot>
      </div>
    </div>
  </div>
</template>
<script>

import {numeric} from "vee-validate/dist/rules.esm";

export default {
  props:{
    type:{
      type: String,
      description: 'tipo do veículo'
    },
    name:{
      type: String,
      description: 'Nome do grupo'
    },
    description:{
      type: String,
      description: 'Descrição'
    },
    range:{
      type: Number,
      description: "Media de preço"
    },
    img:{
      type:String,
      description: "Caminho da IMG"
    }
  },
  methods:{

  }
}
</script>

<style scoped>

</style>
