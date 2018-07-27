<template>
  <component
    :is="link ? 'router-link' : 'button'"
    :to="localizedRoute(link)"
    class="button-outline no-outline py18 bg-cl-transparent h4 no-underline sans-serif align-center lh20"
    :class="{
      light : color === 'light', 'brdr-white' : color === 'light', 'cl-white' : color === 'light',
      dark : color === 'dark', 'brdr-mongoose' : color === 'dark', 'cl-mongoose' : color === 'dark'
    }"
  >
    <slot>
      Button
    </slot>
  </component>
</template>

<script>
import focusClean from 'theme/components/theme/directives/focusClean'

export default {
  name: 'ButtonOutline',
  directives: { focusClean },
  props: {
    color: {
      type: String,
      required: true
    },
    link: {
      type: String,
      default: null,
      required: false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~theme/css/variables/colors';
@import '~theme/css/helpers/functions/color';
$mongoose: color(mongoose);
$cod-gray: color(cod-gray);
$white: color(white);

  .button-outline {
    border: 2px solid;
    min-width: 262px;
  }
  .dark {
    &:hover,
    &:focus {
      color: $cod-gray;
      background: $mongoose;
      border-color: $mongoose;
    }
  }
  .light {
    &:hover,
    &:focus {
      color: $cod-gray;
      background: $white;
      border-color: $white;
    }
  }
</style>
