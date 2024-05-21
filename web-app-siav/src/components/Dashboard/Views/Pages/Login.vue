<template>
  <div class="login-page">
    <div class="wrapper wrapper-full-page">
      <div class="full-page login-page section-image">
        <!--   you can change the color of the filter page using: data-color="blue | azure | green | orange | red | purple" -->
        <div class="content">
          <div class="container">
            <div class="col-lg-4 col-md-6 ml-auto mr-auto">
              <form @submit.prevent="login">
                <card type="login">
                  <h3 slot="header" style="font-weight: bold;" class="header text-center">SIAV</h3>

                  <fg-input v-model="form.username" addon-left-icon="nc-icon nc-single-02"
                            placeholder="Email / CPF-CNPJ"></fg-input>

                  <fg-input v-model="form.password" addon-left-icon="nc-icon nc-key-25" placeholder="Senha"
                            type="password"></fg-input>

                  <br>

                  <p-button native-type="submit" slot="footer" type="info" round block class="mb-3">Entrar</p-button>
                </card>
              </form>
            </div>
          </div>
        </div>
        <div class="full-page-background" style="background-image: url(static/img/background/siav-background-image.jpeg) "></div>
      </div>
    </div>
  </div>
</template>
<script>
  import { Card, Checkbox, Button } from 'src/components/UIComponents';
  import AppNavbar from './Layout/AppNavbar'
  import AppFooter from './Layout/AppFooter'
  import swal from "sweetalert2";

  export default {
    components: {
      Card,
      [Checkbox.name]: Checkbox,
      [Button.name]: Button
    },
    methods: {
      toggleNavbar() {
        document.body.classList.toggle('nav-open')
      },
      closeMenu() {
        document.body.classList.remove('nav-open')
        document.body.classList.remove('off-canvas-sidebar')
      },
      login() {
        // handle login here
      },
      getResponseMessage() {
        let mensagem = {
          content: this.message
        }
        if(this.message){
          this.loading = true
          fetch('http://127.0.0.1:8000/get-message', {
            method: "POST",
            body: JSON.stringify({'content': this.message})
          })
            .then((response) => {
              response.json().then((data) => {
                this.responseMessage = data.content.text.value
                this.loading = false
              });
            })
            .catch((err) => {
              console.error(err);
              this.loading = false
            });
        }else{
          this.loading = false
          swal({
            title: "Você deve colocar uma mensagem",
            type: "error"
          })
        }
      },
    },
    data() {
      return {
        form: {
          username: '',
          password: ''
        },
        message: null,
        responseMessage: null,
        loading: false
      }
    },
    beforeDestroy() {
      this.closeMenu()
    }
  }
</script>
<style>
</style>
