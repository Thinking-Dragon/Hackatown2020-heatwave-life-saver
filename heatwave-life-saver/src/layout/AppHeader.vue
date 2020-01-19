<template>
    <header class="header-global">
        <base-nav class="navbar-main" transparent type="" effect="light" expand>
            <router-link slot="brand" class="navbar-brand mr-lg-5" to="/">
                <img src="img/brand/logo5.png" alt="logo">
            </router-link>

            <div class="row" slot="content-header" slot-scope="{closeMenu}">
                <div class="col-6 collapse-brand">
                    <a href="https://demos.creative-tim.com/vue-argon-design-system/documentation/">
                        <img src="img/brand/logo5 - blue.png">
                    </a>
                </div>
                <div class="col-6 collapse-close">
                    <close-button @click="closeMenu"></close-button>
                </div>
            </div>

            <ul class="navbar-nav navbar-nav-hover align-items-lg-center">
                <a slot="title" href="/home" class="nav-link" data-toggle="dropdown" role="button">
                        <i class="fa fa-home" aria-hidden="true"></i>
                        <span class="nav-link-inner--text">Accueil</span>
                </a>
                <a slot="title" href="#/map" class="nav-link" data-toggle="dropdown" role="button">
                        <i class="fa fa-map" aria-hidden="true"></i>
                        <span class="nav-link-inner--text">Guide</span>
                </a>
                 <a slot="title" href="#/alertes" class="nav-link" data-toggle="dropdown" role="button">
                        <i class="fa fa-exclamation-triangle" aria-hidden="true"></i>
                        <span class="nav-link-inner--text">Alertes</span>
                </a>
            </ul>
           
        </base-nav>
    </header>
</template>
<script>
import BaseNav from "@/components/BaseNav";
import BaseDropdown from "@/components/BaseDropdown";
import CloseButton from "@/components/CloseButton";

export default {
  components: {
    BaseNav,
    CloseButton,
    BaseDropdown
  }
};
</script>
<style>
</style>
