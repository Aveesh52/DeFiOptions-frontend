<template>
  <!-- Sidebar -->
  <ul class="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion" :class="{ toggled: getCollapsedStatus }" id="accordionSidebar">

      <!-- Sidebar - Brand -->
      <router-link to="/" style="text-decoration: none">
      <a class="sidebar-brand d-flex align-items-center justify-content-center" href="/">
          <div class="sidebar-brand-icon">
              <i class="icon-defioptions"></i>
          </div>
          <div class="sidebar-brand-text mx-3">DeFi Options</div>
      </a>
      </router-link>

      <!-- Divider -->
      <hr class="sidebar-divider my-0">

      <!-- Nav Item - Main Page -->
      <router-link to="/" style="text-decoration: none">
        <li class="nav-item" :class="{active:this.$route.name === 'home'}">
            <a class="nav-link" href="/">
                <i class="fas fa-fw fa-home"></i>
                <span>Main page</span></a>
        </li>
      </router-link>
      <!-- END Nav Item - Main Page -->

      <!-- Nav Item - Trade -->
      <router-link to="/trade" style="text-decoration: none" v-if="isUserConnected">
        <li class="nav-item" :class="{active:this.$route.name === 'trade'}">
            <a class="nav-link" href="/trade">
                <i class="fas fa-fw fa-exchange-alt"></i>
                <span>Trade</span></a>
        </li>
      </router-link>
      <!-- END Nav Item - Trade -->

      <!-- Nav Item - Invest -->
      <router-link to="/invest" style="text-decoration: none" v-if="isUserConnected">
        <li class="nav-item" :class="{active:this.$route.name === 'invest'}">
            <a class="nav-link" href="/invest">
                <i class="fas fa-fw fa-wallet"></i>
                <span>Invest</span></a>
        </li>
      </router-link>
      <!-- END Nav Item - Invest -->

      <!-- Nav Item - Portfolio -->
      <router-link to="/portfolio" style="text-decoration: none" v-if="isUserConnected">
        <li class="nav-item" :class="{active:this.$route.name === 'portfolio'}">
            <a class="nav-link" href="/portfolio">
                <i class="fas fa-fw fa-user"></i>
                <span>Portfolio</span></a>
        </li>
      </router-link>
      <!-- END Nav Item - Portfolio -->

      <!-- Divider -->
      <hr class="sidebar-divider d-none d-md-block">

      <!-- Sidebar Toggler (Sidebar) -->
      <div class="text-center d-none d-md-inline">
          <button class="rounded-circle border-0" id="sidebarToggle" @click="toggleSidebar"></button>
      </div>

      <!-- Sidebar Message -->
      <div class="sidebar-card" :class="{'bg-danger': !isCurrentChainSupported}">
          <p class="text-center mb-2">
            <span v-if="getChainName">Your currently selected chain is <strong>{{getChainName}}</strong>.</span>

            <span v-if="!isCurrentChainSupported"> 
              This chain is <strong>not supported</strong> by DeFiOptions! 
              Please switch to one of the supported chains: {{getSupportedChainsString}}.
            </span>

            <span v-if="!getChainName">
              Please <a href="#" @click="connectWeb3Modal">connect</a> with MetaMask
              or some other Ethereum wallet.
            </span>
          </p>
      </div>

  </ul>
  <!-- End of Sidebar -->
</template>

<script>
import { mapGetters, mapActions } from "vuex";

export default {
  name: "Sidebar",
  computed: {
    ...mapGetters("accounts", ["getChainName", "isUserConnected", "getWeb3Modal", "isCurrentChainSupported", "getSupportedChains"]),
    ...mapGetters("sidebar", ["getCollapsedStatus"]),

    getSupportedChainsString() {
      return String(this.getSupportedChains).replace("[", "").replace("]", "").replace(",", ", ");
    }
  },
  created() {
    this.$store.dispatch("accounts/initWeb3Modal");
    this.$store.dispatch("accounts/ethereumListener");
  },
  methods: {
    ...mapActions("accounts", ["connectWeb3Modal", "disconnectWeb3Modal"]),

    toggleSidebar() {
      const el = document.body;

      if (this.getCollapsedStatus === true) {
        this.$store.dispatch("sidebar/sidebarCollapsedFalse");
        el.classList.remove("sidebar-toggled");
      } else {
        this.$store.dispatch("sidebar/sidebarCollapsedTrue");
        el.classList.add("sidebar-toggled");
      }
    }
  }
}
</script>
