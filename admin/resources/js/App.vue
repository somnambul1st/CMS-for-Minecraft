<template>
  <v-app
	:dark="darkMode"
	id="inspire"
	:class="[{ 
		'box-layout': boxLayout, 
		'collapse-sidebar': collapseSidebar, 
		'rtl-layout': rtlLayout
	}]"
  >
      <router-view></router-view>
  </v-app>
</template>

<script>
import { mapGetters } from "vuex";

import {LOAD_USER} from "./api";

export default {
    data() {
    return {
      animation: {
        enter: {
          opacity: [1, 0],
          translateX: [0, -300],
          scale: [1, 0.2]
        },
        leave: {
          opacity: 0,
          height: 0
        }
     }
    };
  },
  created(){
      this.$store.dispatch(LOAD_USER);
  },
  mounted() {
    if (this.selectedLocale.locale === "he" || this.selectedLocale.locale === "ar") {
      this.$store.dispatch("rtlLayout");
    }
  },
  computed: {
    ...mapGetters([
      "darkMode",
      "collapseSidebar",
      "boxLayout",
      "rtlLayout",
      "selectedLocale"
    ])
  }
};
</script>
