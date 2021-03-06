<template>
  <header>
    <div class="logo-wrapper">
      <div class="bg-white">
        <img id="logo" src="../assets/logo.svg" alt />
      </div>
    </div>
    <div class="error-message" v-show="statusInt==4">Disconnected</div>
    <div id="status-wrapper">
      <Saved ref="saved" v-show="statusInt==0"></Saved>
      <Unsaved ref="unsaved" v-show="statusInt==1"></Unsaved>
      <Error ref="error" v-show="statusInt==4"></Error>
      <md-tooltip v-show="statusInt!=4">{{ $t("lih.savedtocloud") }}</md-tooltip>
      <md-tooltip v-show="statusInt==4">{{ $t("lih.couldntsave") }}</md-tooltip>
    </div>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-title>{{ $t("lih.addtohome") }}</md-dialog-title>
      <div class="md-dialog-content">
        {{ $t("lih.addtohomeguide") }}
        <section class="ios" >
          <h2>iOS</h2>
          <div class="guide-content">
            <img src="../assets/ios-share-icon.svg" alt=""> {{$t("lih.iosguide")}}
          </div>
        </section>
        <section class="android" >
          <h2>Android</h2>
          <div class="guide-content">
            <md-icon>more_vert</md-icon> {{$t("lih.androidguide")}}
          </div>
        </section>
        <section class="pc" >
          <h2>PC (Chrome)</h2>
          <div class="guide-content">
            <md-icon>more_vert</md-icon> {{$t("lih.pcguide")}}
          </div>
        </section>
      </div>
      <md-dialog-actions>
        <md-button @click="showDialog = false">{{ $t("lih.closedialog") }}</md-button>
      </md-dialog-actions>
    </md-dialog>
    <md-menu class="dropdown" md-size="medium" md-direction="bottom-start">
      <md-button class="md-icon-button" md-menu-trigger>
        <md-icon>more_vert</md-icon>
      </md-button>
      <md-menu-content class="md-elevation-1">
        <md-menu-item id="addtohome" @click="showDialog = true">{{ $t("lih.addtohome") }}</md-menu-item>
        <md-menu-item id="aboutme">
          <a href="https://twitter.com/rtr_dnd">{{ $t("lih.aboutme") }}</a>
        </md-menu-item>
        <md-menu-item id="language" v-on:click="lang">{{ $t("lih.language") }}</md-menu-item>
        <md-menu-item id="reload" onclick="window.location.reload(true)">{{ $t("lih.reload") }}</md-menu-item>
        <md-menu-item id="version">{{ $t("lih.version") }} 1.0.0</md-menu-item>
        <md-menu-item id="signout" onclick="signout()">{{ $t("lih.signout") }}</md-menu-item>
        <md-menu-item id="delete" onclick="deleteAccount()">{{ $t("lih.deleteaccount") }}</md-menu-item>
      </md-menu-content>
    </md-menu>
  </header>
</template>

<script>
import Vue from "vue";
import {
  MdButton,
  MdContent,
  MdMenu,
  MdIcon,
  MdList,
  MdTooltip,
  MdDialog
} from "vue-material/dist/components";
import "vue-material/dist/vue-material.min.css";
import Saved from "./Saved";
import Unsaved from "./Unsaved";
import ErrorAnim from "./Error";

Vue.use(MdButton);
Vue.use(MdContent);
Vue.use(MdMenu);
Vue.use(MdIcon);
Vue.use(MdList);
Vue.use(MdTooltip);
Vue.use(MdDialog);
Vue.component("Saved", Saved);
Vue.component("Unsaved", Unsaved);
Vue.component("Error", ErrorAnim);

var statusInt = 0; // 0:saved 1:unsaved 4:error

export default {
  name: "LoggedInHeader",
  components: {
    Saved,
    Unsaved,
    ErrorAnim
  },
  props: {
    animationState: {
      required: true
    }
  },
  data: function() {
    return {
      statusInt: 0,
      initialStatus: 1,
      showDialog: false
    };
  },
  watch: {
    animationState: function(newState, oldState) {
      if (newState == "saved") {
        this.statusInt = 0;
        this.$refs.saved.restart();
      } else if (newState == "unsaved") {
        this.statusInt = 1;
        this.$refs.unsaved.restart();
      } else if (newState == "error") {
        if (this.initialStatus == 1) {
          this.initialStatus = 0;
        } else {
          this.statusInt = 4;
          this.$refs.error.restart();
        }
      }
    }
  },
  methods: {
    lang: function() {
      if (this.$i18n.locale === "ja") {
        window.location.pathname = "/en";
      } else {
        window.location.pathname = "/ja";
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.md-menu-content {
  z-index: 1000;
  background-color: #fff;
  // box-shadow: 0 3px 1px -2px rgba(0,0,0,.2), 0 2px 2px 0 rgba(0,0,0,.14), 0 1px 5px 0 rgba(0,0,0,.12) !important;
  box-shadow: none !important;
  border-radius: 5px !important;
  border: 1px solid;
  border-color: rgba(0, 0, 0, 0.1);
}
.md-icon-button {
  color: rgba(0, 0, 0, 0.25);
}
#aboutme {
  a {
    text-decoration: none;
    color: rgba(0, 0, 0, 0.6);
    &:hover {
      text-decoration: !important;
    }
  }
}
header {
  width: calc(100vw - 48px);
  position: fixed;
  margin: 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-sizing: border-box;
  z-index: 10;
  @media (max-width: 600px) {
    background-color: rgba(255, 255, 255, 0.95);
    margin: 12px;
    width: calc(100vw - 24px);
  }
  @media (min-width: 600px) {
    pointer-events: none;
  }
}
li {
  font-size: 16px;
  list-style: none;
  color: rgba(0, 0, 0, 0.6);
  cursor: pointer;
  font-size: 18px;
}
.bg-white {
  display: inline-block;
  background-color: rgba(255, 255, 255, 0.95);
  @media (max-width: 600px) {
    background-color: transparent;
  }
}
#logo {
  margin: 16px;
  height: 32px;
  @media (max-width: 600px) {
    margin: 20px;
  }
}
.logo-wrapper {
  flex-grow: 1;
}
.md-tooltip {
  font-size: 12px;
  background-color: rgba(255, 255, 255, 0.95) !important;
  color: rgba(0, 0, 0, 0.5);
  border: 1px solid !important;
  border-color: rgba(0, 0, 0, 0.1) !important;
}
.error-message {
  pointer-events: auto;
  color: #c62828;
}
#status-wrapper {
  pointer-events: auto;
}
.dropdown {
  pointer-events: auto;
}
</style>
