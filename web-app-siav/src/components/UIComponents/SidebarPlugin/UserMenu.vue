<template>
  <div class="user">
    <div class="photo">
      <img src="static/img/user-png.png" alt="user avatar"/>
    </div>
    <div class="info">
      <a data-toggle="collapse" :aria-expanded="!isClosed" @click="toggleMenu" href="#">
         <span>
           {{ clienteNome }}
        </span>
      </a>
    </div>
  </div>
</template>

<script>
import { CollapseTransition } from 'vue2-transitions';

export default {
  components: {
    CollapseTransition
  },
  data() {
    return {
      isClosed: true
    };
  },
  computed: {
    clienteNome() {
      const cliente = localStorage.getItem('cliente');
      if (cliente !== null) {
        const clienteObj = JSON.parse(cliente);
        return clienteObj.clienteNome || 'Usuário';
      }
      return 'Usuário';
    }
  },
  methods: {
    toggleMenu() {
      this.isClosed = !this.isClosed;
    }
  }
};
</script>

<style scoped>
.nav.nav-menu {
  margin-top: 0;
}
</style>
