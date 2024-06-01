<template>
  <div class="login-page">
    <div class="wrapper wrapper-full-page">
      <div class="full-page login-page section-image">
        <div class="content">
          <div class="container">
            <div class="col-lg-4 col-md-6 ml-auto mr-auto">
              <form @submit.prevent="login" v-loading="loading">
                <card type="login" style="text-align: center">
                  <h3 slot="header" style="font-weight: bold;" class="header text-center">SIAV</h3>

                  <fg-input v-model="form.login" addon-left-icon="nc-icon nc-single-02"
                            placeholder="Login"></fg-input>

                  <fg-input v-model="form.senha" addon-left-icon="nc-icon nc-key-25" placeholder="Senha"
                            type="password"></fg-input>
                  <br>
                  <p-button @click="login" native-type="submit" type="info" round block class="mb-3">Entre</p-button>
                  <p>OU</p>
                  <RouterLink style="font-weight: bold" to="/register">Crie sua conta</RouterLink>
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
import swal from "sweetalert2";

export default {
  components: {
    Card,
    [Checkbox.name]: Checkbox,
    [Button.name]: Button
  },
  methods: {
    async login() {
      this.loading = true;
      try {
        const response = await fetch('http://localhost:8081/auth/login', {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.form)
        });
        const data = await response.json();
        localStorage.setItem('cliente', JSON.stringify(data));
        this.$router.push('/admin/overview');
      } catch (err) {
        console.error('Login Falhou:', err);
        swal("Erro", "Login Falhou", "error");
      } finally {
        this.loading = false;
      }
    }
  },
  data() {
    return {
      form: {
        login: '',
        senha: ''
      },
      loading: false
    };
  }
};
</script>

<style>
</style>
