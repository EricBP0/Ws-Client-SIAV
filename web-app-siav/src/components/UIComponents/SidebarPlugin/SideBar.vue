<template>
  <div class="sidebar"
       :data-color="backgroundColor"
       :data-active-color="activeColor">
    <div class="logo">
      <a class="simple-text logo-mini"
         aria-label="sidebar mini logo"
         href="http://localhost:8080">
          <div class="logo-img">
              <img :src="logo" alt="">
          </div>
      </a>
      <a class="simple-text logo-normal"
         href="http://localhost:8080">
          {{ title }}
      </a>
    </div>
    <div class="sidebar-wrapper" ref="sidebarScrollArea">
      <slot>
        <UserMenu/>
      </slot>
      <ul class="nav">
        <slot name="links">
          <sidebar-item v-for="(link, index) in sidebarLinks"
                        :key="link.name + index"
                        :link="link">

            <sidebar-item v-for="(subLink, index) in link.children"
                          :key="subLink.name + index"
                          :link="subLink">
            </sidebar-item>
          </sidebar-item>
        </slot>
      </ul>
      <div style="position: absolute; bottom: 20px;width: 100%;text-align: center">
        <p-button type="danger" @click="logout">Deslogar</p-button>
      </div>
    </div>
  </div>
</template>
<script>
  import 'perfect-scrollbar/dist/css/perfect-scrollbar.css'
  import UserMenu from "@/components/UIComponents/SidebarPlugin/UserMenu.vue";
  export default {
    components: {UserMenu},
    props: {
      title: {
        type: String,
        default: 'SIAV',
        description: 'Sidebar title'
      },
      backgroundColor: {
        type: String,
        default: 'white',
        validator: (value) => {
          let acceptedValues = ['white', 'brown', 'black']
          return acceptedValues.indexOf(value) !== -1
        },
        description: 'Sidebar background color (white|brown|black)'
      },
      activeColor: {
        type: String,
        default: 'success',
        validator: (value) => {
          let acceptedValues = ['primary', 'info', 'success', 'warning', 'danger']
          return acceptedValues.indexOf(value) !== -1
        },
        description: 'Sidebar active text color (primary|info|success|warning|danger)'
      },
      logo: {
        type: String,
        default: 'static/img/logo-siav.png',
        description: 'Sidebar Logo SIAV'
      },
      sidebarLinks: {
        type: Array,
        default: () => [],
        description: 'Sidebar links. Can be also provided as children components (sidebar-item)'
      },
      autoClose: {
        type: Boolean,
        default: true
      }
    },
    provide() {
      return {
        autoClose: this.autoClose
      }
    },
    methods: {
      async initScrollBarAsync () {
        let isWindows = navigator.platform.startsWith('Win');
        if(!isWindows) {
          return;
        }
        const PerfectScroll = await import('perfect-scrollbar')
        PerfectScroll.initialize(this.$refs.sidebarScrollArea)
      },
      logout() {
        localStorage.removeItem('cliente');
        window.location.reload();
      }
    },
    mounted () {
      this.initScrollBarAsync()
    },
    beforeDestroy () {
      if (this.$sidebar.showSidebar) {
        this.$sidebar.showSidebar = false
      }
    }
  }

</script>
<style>
  @media (min-width: 992px) {
    .navbar-search-form-mobile,
    .nav-mobile-menu{
      display: none;
    }
  }
</style>
