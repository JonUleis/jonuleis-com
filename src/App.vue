<template>
  <div
    id="app"
    class="app"
    :class="{
      'app--desktop': !isMobile().any,
      'app--dark': dark,
      'app--intro': intro,
      'app--initial': intro === 3,
    }"
  >
    <div class="wrapper">
      <header
        class="header"
        :class="{ 'header--intro': intro > 1, 'header--animating': intro }"
      >
        <img
          svg-inline
          id="logo"
          :class="{ logo, hide: logo.hide, drawing: logo.drawing }"
          ref="logo"
          src="./assets/logo.svg"
          alt="Jon Uleis"
        />
        <button
          class="header__light"
          @click="toggleDark"
          :data-hover="dark ? 'Lights On' : 'Lights Off'"
          :aria-label="dark ? 'Dark Mode Off' : 'Dark Mode On'"
        >
          <i
            class="fa fa-lightbulb-o"
            aria-hidden="true"
            :title="dark ? 'Dark Mode Off' : 'Dark Mode On'"
          />
        </button>
        <div class="header__links">
          <a
            v-for="info in contact"
            class="contact-link"
            target="_blank"
            rel="noopener"
            :href="info.link"
            :data-hover="info.text"
            :aria-label="info.text"
            :key="info.text"
            ><i class="fa" :class="info.icon" aria-hidden="true" />
          </a>
        </div>
      </header>
      <section class="section section--info">
        <div class="info">
          <div class="info__photo">
            <img src="./assets/jon.jpg" alt="My head" />
          </div>
          <div class="info__description" ref="description">
            <p>
              How's it going? I'm Jon, a native New Yorker who loves an
              everything bagel and a pretty website. As Lead Front-End Developer
              at
              <a href="http://ralphandco.com/" target="_blank" rel="noopener"
                >Ralph NYC</a
              >, I've headed up a team in creating delightful,
              <a
                href="https://www.emmys.com/bios/ralph-interactive"
                target="_blank"
                >Emmy Award-winning</a
              >
              interactive websites for some of the best names in the
              entertainment industry. At home, I freelance and make sites for
              fun and various clients, including some excellent rock bands.
            </p>
            <p>
              My passion for the web began the day I touched a keyboard and is
              still going strong as ever. I work daily to make the web nicer one
              pixel at a time, specializing in front-end and back-end
              development with a love for Photoshop and everything in between.
              In my free time you can usually catch me at a concert taking
              photos or videos for my
              <a href="https://www.youtube.com/capitalq" target="_blank"
                >YouTube channel</a
              >. If you'd like to reach out for a freelance project or anything
              else, please <a href="mailto:hi@jonuleis.com">send an email</a>.
              Thanks!
            </p>
          </div>
        </div>
      </section>
      <div class="section__title">
        <h2>Web Development</h2>
      </div>
      <section class="section section--tiles">
        <div
          v-for="site in work"
          class="site-tile"
          :class="[
            { active: selected === site.name },
            `site-tile--${site.name}`,
          ]"
          @click="isMobile().any ? (selected = site.name) : null"
          @mouseenter="tileOver"
          @mouseleave="tileOut"
          :key="site.name"
        >
          <div
            class="site-bg"
            v-lazy:background-image="
              require(`@/assets/site-image-${site.name}.jpg`)
            "
          />
          <div class="site-logo-gate gate-left" />
          <div class="site-logo-gate gate-right" />
          <div class="site-caption">
            {{ site.copy }}
            <a
              v-if="site.url"
              class="site-link"
              :href="site.url"
              target="_blank"
              rel="noopener"
              >{{ site.link }}</a
            >
            <a v-else class="site-link archived">{{ site.link }} (archived)</a>
          </div>
        </div>
      </section>
      <div class="section__title">
        <h2>Concert Photography</h2>
      </div>
      <section class="section section--tiles section--photos">
        <div
          v-for="site in photos"
          class="site-tile photo-tile"
          :class="`site-tile--${site.name}`"
          :key="site.name"
        >
          <a
            class="site-photo-link"
            :href="site.url"
            target="_blank"
            rel="noopener"
            >{{ site.link }}</a
          >
          <div
            class="site-photo"
            v-lazy:background-image="require(`@/assets/photo-${site.name}.jpg`)"
          />
          <div class="site-caption">
            {{ site.copy }}
          </div>
        </div>
      </section>
      <div class="section__title">
        <h2>Find Me &amp; Follow Me</h2>
      </div>
      <section class="section">
        <div class="social">
          <a
            v-for="info in contact"
            class="contact-link"
            target="_blank"
            rel="noopener"
            :href="info.link"
            :data-hover="info.text"
            :aria-label="info.text"
            :key="info.text"
            ><i class="fa" :class="info.icon" aria-hidden="true" />
          </a>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import data from "./data.js";
import isMobile from "ismobilejs";
import Vivus from "vivus";

export default {
  name: "app",
  mounted() {
    if (localStorage && localStorage.dark) {
      this.dark = JSON.parse(localStorage.dark);
    } else if (
      window.matchMedia &&
      window.matchMedia("(prefers-color-scheme: dark)").matches
    ) {
      this.dark = true;
    }
    new Vivus(
      "logo",
      {
        duration: 130,
        delay: 120,
        animTimingFunction: Vivus.EASE,
        onReady: () => {
          let logo = this.$refs.logo;
          this.intro--;
          this.sizeLogo = () => {
            logo.style.transition = "none";
            logo.style.transform = `translate(${
              window.innerWidth / 2 - logo.clientWidth / 2
            }px, ${(window.innerHeight - 32) / 2 - logo.clientHeight / 2}px)`;
            logo.style.width = `${logo.clientWidth}px`;
            logo.style.height = `${logo.clientHeight}px`;
            logo.style.transition = "";
          };
          this.sizeLogo();
          window.addEventListener("resize", this.sizeLogo);
          this.logo.hide = false;
        },
      },
      () => {
        let logo = this.$refs.logo;
        window.removeEventListener("resize", this.sizeLogo);
        this.logo.drawing = false;
        logo.style.transform = `translate(${
          this.$refs.description.getBoundingClientRect().left
        }px, 0)`;
        logo.style.width = "";
        logo.style.height = "";
        this.intro--;
        this.introDone = (event) => {
          if (event.propertyName == "transform") {
            this.intro = 0;
            logo.style.transform = "";
            logo.removeEventListener("transitionend", this.introDone);
          }
        };
        logo.addEventListener("transitionend", this.introDone);
      }
    );
  },
  methods: {
    tileOver(e) {
      e.target.classList.add("hover");
      setTimeout(() => {
        if (
          e.target.classList.contains("hover") &&
          !e.target.classList.contains("closing")
        ) {
          e.target.classList.add("open");
        }
      }, 700);
    },
    tileOut(e) {
      e.target.classList.remove("hover");
      e.target.classList.remove("open");
      e.target.classList.add("closing");
      setTimeout(() => {
        e.target.classList.remove("closing");
      }, 600);
    },
    toggleDark() {
      this.dark = !this.dark;
      localStorage.dark = this.dark;
      window.navigator.vibrate([25, 100, 50]);
    },
  },
  data() {
    return {
      isMobile,
      contact: data.contact,
      work: data.work,
      photos: data.photos,
      intro: 3,
      selected: null,
      dark: false,
      logo: {
        hide: true,
        drawing: true,
      },
    };
  },
};
</script>

<style lang="scss">
@import "styles/main";
</style>
