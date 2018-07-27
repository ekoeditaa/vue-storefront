<template>
  <section class="offers container mt40 px15">
    <div class="row">
      <div
        class="col-xs-12 col-sm-6"
        v-for="(banner, index) in banners.mainBanners"
        :key="index"
      >
        <router-link :to="localizedRoute(banner.link)">
          <div class="p40 align-center">
            <p class="subtitle cl-secondary">
              {{ banner.subtitle }}
            </p>
            <h2 class="title my20 cl-secondary">
              {{ banner.title }}
            </h2>
            <div class="offer">
              <img v-lazy="banner.image">
            </div>
          </div>
        </router-link>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import promotedOffers from 'theme/resource/promoted_offers.json'

export default {
  name: 'PromotedOffers',
  computed: {
    ...mapGetters({
      banners: 'promoted/getPromotedOffers'
    })
  },
  created () {
    this.updatePromotedOffers(promotedOffers)
  },
  methods: {
    ...mapActions({
      updatePromotedOffers: 'promoted/updatePromotedOffers'
    })
  }
}
</script>

<style lang="scss" scoped>
  .offer {
    opacity: 0.9;
    transition: 0.3s all;
    position: relative;

    &:before {
      content: "";
      display: block;
      padding-bottom: 100%;
    }

    img {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      margin: auto;
      max-width: 80%;

      @media only screen and (min-width: 1200px) {
        max-height: 100%;
      }

    }

    &:hover {
      opacity: 1;
    }

  }
  .title {
    @media (max-width: 767px) {
      font-size: 36px;
    }
  }
  .subtitle {
    @media (max-width: 767px) {
      font-size: 18px;
    }
  }
</style>
