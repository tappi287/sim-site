<template>
  <footer class="fill-gray-lighter xs-text-6 md-text-5">
    <div class="r no-gap">
      <div v-if="pagination.active" class="c-12 xs-text-left xs-p2">
        <lazy-bael-pagination :pagination="pagination" />
      </div>
      <div
        class="xs-text-left xs-p2 xs-border xs-border-right-none xs-border-bottom-none"
        :class="signupAboutSize"
      >
        <div class="item">
          <div v-if="siteDescription" class="footer__heading xs-mb2">About</div>
          <p v-if="siteDescription">{{siteDescription}}</p>
        </div>
      </div>
      <div
        v-if="signupBoolean"
        class="c-25 xs-text-left xs-p2 xs-border xs-border-right-none xs-border-bottom-none"
      >
        <lazy-send-grid />
      </div>
      <div
        class="xs-text-left xs-p2 xs-border xs-border-right-none xs-border-bottom-none"
        :class="signupAboutSize"
      >
        <div class="item">
          <div v-if="connectData" class="footer__heading xs-mb2">Connect</div>
          <ul v-if="connectData" class="list-unstyled">
            <li v-for="(c,i) in connectData" :key="i">
              <a :href="c.url">{{c.name}}</a>
            </li>
          </ul>
        </div>
      </div>
      <div class="xs-text-left xs-p2 xs-border xs-border-right-none xs-border-bottom-none" :class="signupAboutSize">
        <div class="item">
          <div class="footer__heading xs-mb2">Apps</div>

          <a href="/page/rF2-settings-widget">
            <img
              style="height:29px;width:auto;"
              src="/images/rfW_banner.svg"
              title="rFactor 2 Settings Widget"
            />
          </a>
          <a href="/page/simmon">
            <img
              style="height:29px;width:auto;"
              src="/images/sw_banner.svg"
              title="SimMon Application Profiler"
            />
          </a>
        </div>
      </div>
      <div class="c-12 xs-text-left xs-p2 xs-border xs-border-right-none xs-border-bottom-none">
        <div class="item xs-text-6">
          <a href="/page/about">SIM SITE - Attributions and Source.</a>
          v<span class="small-txt" v-if="version">{{version}}</span> If not otherwise noticed artworks and content
          (C) 2021 <a href="https://www.linkedin.com/in/stefan-tapper-723b6635/">Stefan Tapper</a>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {},
  computed: {
    pagination() {
      return this.$store.state.pagination;
    },
    version() {
      return process.env.BAEL_VERSION || false;
    },
    signupAboutSize: function () {
      return {
        "c-25": this.signupBoolean,
        "c-4": !this.signupBoolean,
      };
    },
    connectData() {
      return this.$store.state.connect;
    },
    siteDescription() {
      return this.$store.state.info.sitedescription;
    },
    signupBoolean() {
      return this.$store.state.info.emailsignup;
    },
  },
};
</script>

<style>
.text-input {
  max-width: 100%;
}
.small-txt {
  font-size: 80%;
}
footer .item:hover .footer__heading {
  color: rgba(119, 119, 119, 0.6);
  transition: 0.2s color ease-in-out;
}
.footer__heading {
  max-width: 100%;
  pointer-events: none;
  color: rgba(119, 119, 119, 0.4);
  font-weight: 800;
  font-size: 85%;
  transition: 0.1s color ease-in-out;
}
.pagination__button--disabled {
  pointer-events: none;
}
</style>
