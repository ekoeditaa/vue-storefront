<template>
  <transition name="fade" appear>
    <div class="cookie fixed w-100 bg-cl-accent cl-accent " v-if="isOpen">
      <div class="container px15">
        <div class="row between-xs middle-xs">
          <div class="col-xs-10 start-xs">
            <span>
              {{ message }}
            </span>
            <router-link :to="localizedRoute('detailsLink')" :title="detailsLinkText">
              {{ detailsLinkText }}
            </router-link>
          </div>
          <div class="col-xs-2 end-xs">
            <i class="material-icons icon p15 pointer" @click="accept">close</i>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import i18n from 'core/lib/i18n'
export default {
  props: {
    detailsLinkText: {
      type: String,
      default: i18n.t('See details')
    },
    detailsLink: {
      type: String,
      default: '/privacy'
    },
    message: {
      type: String,
      default: i18n.t('We use cookies to give you the best shopping experience.')
    }
  },
  data () {
    return {
      isOpen: false
    }
  },
  methods: {
    accept () {
      this.setVisited()
      this.isOpen = false
    },
    setVisited () {
      this.$store.dispatch('claims/set', {claimCode: 'cookiesAccepted', value: true})
    }
  },
  created () {
    this.$store.dispatch('claims/check', {claimCode: 'cookiesAccepted'}).then((cookieClaim) => {
      if (!cookieClaim) {
        this.isOpen = true
        this.$store.dispatch('claims/set', {claimCode: 'cookiesAccepted', value: false})
      } else {
        this.isOpen = !cookieClaim.value
      }
    })
  }
}
</script>

<style lang="scss" scoped>
@import '~theme/css/variables/colors';
@import '~theme/css/helpers/functions/color';
$color-icon-hover: color(burnt-umber);

.cookie {
  z-index: 2;
  bottom: 0;
}

.icon {
  &:hover,
  &:focus {
    background-color: lighten($color-icon-hover, 5%);
  }
}
</style>
