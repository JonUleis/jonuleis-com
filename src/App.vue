<template lang="pug">
  #app(:class="{'is-desktop': !isMobile().any}")
    object.logo#logo(type="image/svg+xml",:data="require('./assets/logo.svg')",ref="logo")
</template>

<script>
import isMobile from "ismobilejs";
import Vivus from "vivus";

export default {
  name: "app",
  components: {},
  data() {
    return {
      isMobile
    };
  },
  mounted() {
    new Vivus(
      "logo",
      {
        duration: 200,
        delay: 130,
        animTimingFunction: Vivus.EASE,
        onReady: svg => {
          svg.el.classList.remove("hide");
        }
      },

      svg => {
        svg.el.classList.add("loaded");
      }
    );
  }
};
</script>
