<template>
  <div class="layout">


    <header-slot/>

    <slot/>

    <subscription v-if="!$route.path.includes('certificates')" />

    <!-- <footer-slot/> -->

    <client-only>

      <userTracker
        privacyPolicyLink="/privacy-policy"
        classCustom="my-tracker"
        @activateTracker="activateTracker"
      />

    </client-only>


  </div>
</template>

<script>
  export default {

    components: {
      HeaderSlot: () => import('~/components/Header.vue'),
      FooterSlot: () => import('~/components/Footer.vue'),
      Subscription: () => import('~/components/Subscription.vue'),
    },

    methods: {
      activateTracker() {
        if(this.$matomo) {
            this.$matomo && this.$matomo.setConsentGiven();
            this.$matomo && this.$matomo.enableLinkTracking();
            this.$matomo && this.$matomo.trackPageView();
            // window._paq.push(['enableLinkTracking'])
            // window._paq.push(['setConsentGiven']); // _paq.push(['setConsentGiven'])
            // window._paq.push(['trackPageView']);
        }
      }
    },

    mounted() {

      if(this.$route.path.includes('online-courses') || this.$route.path.includes('certificates') || this.$route.path.includes('privacy-policy')) {
        this.$store.commit('TOGGLE_SHOW_HEADER', true)
      }

    }
  }
</script>
