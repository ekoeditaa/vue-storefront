<template>
  <div class="header bg-cl-accent">
    <header
      class="fixed w-100 bg-cl-accent"
      :class="{ 'is-visible': navVisible }"
    >
      <div class="container px15">
        <div class="row between-xs middle-xs" v-if="!isCheckout">
          <div class="col-sm-6 col-xs-4 middle-xs">
            <div>
              <template v-if="!isProductPage">
                <hamburger-icon class="p15 icon cl-accent pointer" v-if="!isProductPage"/>
              </template>
              <template v-else>
                <return-icon class="p15 icon cl-accent pointer" v-if="isProductPage"/>
              </template>
            </div>
          </div>
          <div class="col-xs-4 visible-xs">
            <search-icon class="p15 icon cl-accent pointer" />
          </div>
          <div class="col-sm-6 col-xs-4 end-xs">
            <div class="inline-flex right-icons">
              <search-icon class="p15 icon cl-accent pointer hidden-xs" />
              <compare-icon class="p15 icon cl-accent pointer hidden-xs" />
              <microcart-icon class="p15 icon cl-accent pointer" />
            </div>
          </div>
        </div>
        <div class="row between-xs middle-xs px15 py5" v-if="isCheckout">
          <div class="col-xs-5 col-md-3 middle-xs">
            <div>
              <router-link :to="localizedRoute('/')" class="cl-tertiary links">
                {{ $t('Return to shopping') }}
              </router-link>
            </div>
          </div>
          <div class="col-xs-2 col-md-6 center-xs">
            <logo width="36px" height="41px"/>
          </div>
          <div class="col-xs-5 col-md-3 end-xs">
            <div>
              <a v-if="!currentUser" href="#" @click="gotoAccount" class="cl-tertiary links">
                {{ $t('Login to your account') }}
              </a>
              <span v-else>
                {{ $t('You are logged in as') }} {{ currentUser.firstname }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </header>
    <div class="header-placeholder"/>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import Header from 'core/components/blocks/Header/Header'

import CompareIcon from 'theme/components/core/blocks/Header/CompareIcon'
import HamburgerIcon from 'theme/components/core/blocks/Header/HamburgerIcon'
import Logo from 'theme/components/core/Logo'
import MicrocartIcon from 'theme/components/core/blocks/Header/MicrocartIcon'
import ReturnIcon from 'theme/components/core/blocks/Header/ReturnIcon'
import SearchIcon from 'theme/components/core/blocks/Header/SearchIcon'

export default {
  components: {
    CompareIcon,
    HamburgerIcon,
    Logo,
    MicrocartIcon,
    ReturnIcon,
    SearchIcon
  },
  mixins: [Header],
  data () {
    return {
      productPageRoutes: [
        'product',
        'simple-product',
        'configurable-product',
        'downloadable-product',
        'grouped-product'
      ],
      isCheckout: false,
      isProductPage: false,
      navVisible: true,
      isScrolling: false,
      scrollTop: 0,
      lastScrollTop: 0,
      navbarHeight: 54
    }
  },
  computed: {
    ...mapState({
      isOpenLogin: state => state.ui.signUp,
      currentUser: state => state.user.current
    })
  },
  beforeCreated () {
    if (this.productPageRoutes.includes(this.$route.name)) {
      this.isProductPage = true
    }
  },
  created () {
    if (this.$route.name === 'checkout') {
      this.isCheckout = true
    }
  },
  beforeMount () {
    window.addEventListener('scroll', () => {
      this.isScrolling = true
    })

    setInterval(() => {
      if (this.isScrolling) {
        this.hasScrolled()
        this.isScrolling = false
      }
    }, 250)
  },
  watch: {
    '$route.name': function () {
      if (this.productPageRoutes.includes(this.$route.name)) {
        this.isProductPage = true
      } else {
        this.isProductPage = false
      }

      if (this.$route.name === 'checkout') {
        this.isCheckout = true
        this.menuFixed = true
      } else {
        this.isCheckout = false
        this.menuFixed = false
      }
    }
  },
  methods: {
    gotoAccount () {
      this.$bus.$emit('modal-toggle', 'modal-signup')
    },
    hasScrolled () {
      this.scrollTop = window.scrollY
      if (this.scrollTop > this.lastScrollTop && this.scrollTop > this.navbarHeight) {
        this.navVisible = false
      } else {
        this.navVisible = true
      }
      this.lastScrollTop = this.scrollTop
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~theme/css/variables/colors';
@import '~theme/css/helpers/functions/color';
$color-icon-hover: color(burnt-umber);

header {
  height: 54px;
  top: -54px;
  z-index: 2;
  transition: top 0.2s ease-in-out;
  &.is-visible {
    top: 0;
  }
}
.icon {
  &:hover,
  &:focus {
    background-color: lighten($color-icon-hover, 5%);
  }
}
.right-icons {
  //for edge
  float: right;
}
.header-placeholder {
  height: 54px;
}
.links {
  text-decoration: underline;
}
@media (max-width: 767px) {
  .row.middle-xs {
    margin: 0 -15px;

    &.py5 {
      margin: 0;
    }
  }
  .col-xs-4:first-of-type {
      padding-left: 0;
  }
  .col-xs-4:last-of-type {
      padding-right: 0;
  }
  a, span {
    font-size: 12px;
  }
}
</style>
