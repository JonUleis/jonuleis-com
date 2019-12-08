<template lang="pug">
  #app(:class="{'is-desktop': !isMobile().any}")
    header.header(:class="{'header--intro': intro}")
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
      isMobile,
      intro: true
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
          let logo = this.$refs.logo;
          logo.style.transition = "none";
          logo.style.transform = `translate(${window.innerWidth / 2 -
            logo.clientWidth / 2}px, ${window.innerHeight / 2 -
            logo.clientHeight / 2}px)`;
          logo.style.width = `${logo.clientWidth}px`;
          logo.style.height = `${logo.clientHeight}px`;
          logo.style.transition = "";
          svg.el.classList.remove("hide");
        }
      },
      svg => {
        let logo = this.$refs.logo;
        svg.el.classList.add("loaded");
        logo.style.transform = "";
        logo.style.width = "";
        logo.style.height = "";
        this.intro = false;
      }
    );
  }
};
</script>

<style lang="scss">
@import "styles/main";
</style>
