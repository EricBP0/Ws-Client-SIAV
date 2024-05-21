
<template>
  <div class="wrapper">
    <side-bar type="sidebar" :sidebar-links="$sidebar.sidebarLinks">
      <user-menu></user-menu>
    </side-bar>
    <div class="main-panel">
      <div style="justify-content: center">
        <top-navbar></top-navbar>

        <card-vehicle></card-vehicle>
      </div>
    </div>
  </div>
</template>

<script>

  import CardVehicle from "@/components/Dashboard/Views/Components/Vehicles/CardVehicle.vue";
  import UserMenu from 'src/components/UIComponents/SidebarPlugin/UserMenu.vue'
  import TopNavbar from "@/components/Dashboard/Layout/TopNavbar.vue";

  export default {
    components: {
      CardVehicle,
      TopNavbar,
      UserMenu,
    },
    methods: {
      toggleSidebar () {
        if (this.$sidebar.showSidebar) {
          this.$sidebar.displaySidebar(false)
        }
      }
    }
  }

</script>

<style scoped>

</style>
