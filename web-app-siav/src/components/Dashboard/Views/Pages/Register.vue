<template>
  <div class="register-page">
    <div class="wrapper wrapper-full-page ">
      <div class="full-page register-page section-image" filter-color="black">
        <div class="content">
          <div class="container">
            <div class="row">
              <div class="col-lg-5 col-md-5 ml-auto">
                <info-section class="mt-5"
                              type="primary"
                              title="Sobre a empresa"
                              description="Somos uma plataforma de aluguel de carros digidal com locadoras fisicas, sempre tentando ao maximo atender nossos clientes"
                              icon="nc-icon nc-tv-2">

                </info-section>
              </div>
              <div class="col-lg-4 col-md-6 mr-auto">
                <card type="signup" class="text-center" v-loading="loading">
                  <template slot="header">
                    <h4 class="card-title">Registre-se</h4>
                    <div class="social">
                      <button class="btn btn-icon btn-round btn-twitter">
                        <i class="fa fa-twitter"></i>
                      </button>
                      <button class="btn btn-icon btn-round btn-google">
                        <i class="fa fa-google"></i>
                      </button>
                      <button class="btn btn-icon btn-round btn-facebook">
                        <i class="fa fa-facebook-f"></i>
                      </button>
                      <p class="card-description"> ou seja Clássico </p>
                    </div>
                  </template>

                  <fg-input v-model="form.login" addon-left-icon="nc-icon nc-single-02" placeholder="Login"></fg-input>
                  <fg-input v-model="form.senha" addon-left-icon="nc-icon nc-key-25" type="password" placeholder="Senha"></fg-input>

                  <br>

                  <fg-input v-model="form.nome" addon-left-icon="nc-icon nc-single-02" placeholder="Nome completo..."></fg-input>
                  <fg-input v-model="form.endereco" addon-left-icon="nc-icon nc-world-2" placeholder="Endereço..."></fg-input>
                  <fg-input v-model="form.documento" addon-left-icon="nc-icon nc-cloud-download-93" placeholder="Documento..."></fg-input>
                  <fg-input v-model="form.telefone" addon-left-icon="nc-icon nc-tablet-2" placeholder="Telefone..."></fg-input>
                  <fg-input v-model="form.email" addon-left-icon="nc-icon nc-email-85" placeholder="Email..."></fg-input>
                  <p-checkbox class="text-left">
                    Eu concordo com os
                    <a href="#something">Termos e condições</a>.
                  </p-checkbox>

                  <p-button @click="register" type="info" round>Registrar-se</p-button>
                </card>
              </div>
            </div>
          </div>
        </div>
        <div class="full-page-background"
             style="background-image: url(static/img/background/siav-background-image.jpeg) "></div>
      </div>
    </div>
  </div>
</template>
<script>
  import AppNavbar from './Layout/AppNavbar'
  import AppFooter from './Layout/AppFooter'
  import { Card, Checkbox, Button, InfoSection } from 'src/components/UIComponents';

  export default {
    components: {
      Card,
      AppNavbar,
      AppFooter,
      InfoSection,
      [Checkbox.name]: Checkbox,
      [Button.name]: Button
    },
    data(){
      return {
        loading: false,
        form: {
          login: '',
          senha: '',
          nome: '',
          documento: '',
          endereco: '',
          telefone: '',
          email: '',
        }
      }
    },
    methods: {
      async register() {
        this.loading = true
        console.log(this.form)
        fetch('http://localhost:8081/auth/register', {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.form)
        })
          .then(response => response.json())
          .then(data => {
            localStorage.removeItem('cliente')
            localStorage.setItem('cliente', JSON.stringify(data));
            this.$router.push('/admin/overview');
            this.loading = false;
          })
          .catch(err => {
            console.error('Registrar Falhou:', err);
            this.loading = false;
          });
      },
      toggleNavbar() {
        document.body.classList.toggle('nav-open')
      },
      closeMenu() {
        document.body.classList.remove('nav-open')
        document.body.classList.remove('off-canvas-sidebar')
      }
    },
    beforeDestroy() {
      this.closeMenu()
    }
  }
</script>
<style>
</style>
