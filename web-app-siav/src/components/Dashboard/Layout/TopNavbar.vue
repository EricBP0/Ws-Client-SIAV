<template>
  <navbar v-model="showNavbar">
    <div class="navbar-wrapper">
      <div class="navbar-minimize">
        <button id="minimizeSidebar" class="btn btn-icon btn-round" @click="minimizeSidebar">
          <i class="nc-icon nc-minimal-right text-center visible-on-sidebar-mini"></i>
          <i class="nc-icon nc-minimal-left text-center visible-on-sidebar-regular"></i>
        </button>
      </div>
      <div class="navbar-toggle">
        <navbar-toggle-button @click.native="toggleSidebar">

        </navbar-toggle-button>
      </div>
      <a class="navbar-brand" href="#pablo">SIAV Sistema Integrado de Aluguel de Veículos</a>
    </div>

    <template slot="navbar-menu">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link btn-rotate" title="Logar" href="#/login" >
            <i class="nc-icon nc-single-02"></i>
            <p style="margin-left: 5px">
              <span class="d-lg-none d-md-block">Login</span>
            </p>
          </a>
        </li>
      </ul>
    </template>
  </navbar>
</template>
<script>
  import { Navbar, NavbarToggleButton } from 'src/components/UIComponents'

  export default {
    components: {
      Navbar,
      NavbarToggleButton
    },
    data() {
      return {
        activeNotifications: false,
        showNavbar: false
      }
    },
    methods: {
      capitalizeFirstLetter(string) {
        return string.charAt(0).toUpperCase() + string.slice(1)
      },
      toggleNotificationDropDown() {
        this.activeNotifications = !this.activeNotifications
      },
      closeDropDown() {
        this.activeNotifications = false
      },
      toggleSidebar() {
        this.$sidebar.displaySidebar(!this.$sidebar.showSidebar)
      },
      hideSidebar() {
        this.$sidebar.displaySidebar(false)
      },
      minimizeSidebar() {
        this.$sidebar.toggleMinimize()
      },
      toggleNavbar() {
        this.showNavbar = !this.showNavbar;
      }
    }
  }

</script>
<style>

</style>
