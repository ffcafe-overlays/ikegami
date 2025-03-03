<template>
  <nav class="c-navbar-wrapper">
    <div class="location">
      <time :class="{ empty: e.duration }">
        <span class="m">{{ ~~(e.duration / 60) | pad }}</span><!--
     --><span class="s">{{ (e.duration || 0) % 60 | pad }}</span>
      </time>
      <span v-if="e.zone">
        {{ e.zone }}
      </span>
      <span v-else>
        <span class="versioninfo">
          ikegami {{ version }}
        </span>
        <span class="releasename">
          {{ releasename }}
        </span>
      </span>
    </div>
    <div class="info">
      <span class="rank">{{ rank }}/{{ c.length }}</span><!--
   --><span class="rdps">{{ (e.rdps || 0) | f(show_decimals) }}</span>
    </div>
    <div class="buttons">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 18 24" width="1.5rem" height="1.125rem"
        class="button"
        @click="dropdown_opened = !dropdown_opened">
        <path d="M4,12h2m2,0h2m2,0h2" stroke="#fff" stroke-width="2" />
      </svg>
    </div>
    <ul class="dropdown" v-if="dropdown_opened" @click="dropdown_opened = false">
      <li @click="endEncounter"> {{$t('overlay.dropdown.endEncounter')}} </li>
      <li @click="open('changelog')"> {{$t('overlay.dropdown.changelog')}} </li>
      <li @click="open('settings')"> {{$t('overlay.dropdown.settings')}} </li>
      <li @click="toggleLayoutMode()"> {{$t('overlay.dropdown.layoutMode')}} </li>
    </ul>
    <section class="c-details">
      <article class="details-group" v-if="theme === 'minimal'">
        <row
          title=""
          :value="e.zone" />
        <hr />
      </article>
      <article class="details-group dps">
        <row
          :title="$t('stats.rdps')"
          :value="e.rdps | decimal" />
        <row
          :title="$t('stats.1m')"
          :value="e.dps1m | decimal" />
        <row
          :title="$t('stats.max')"
          :value="e.max" />
        <hr />
      </article>

      <article class="details-group healer">
        <row
          :title="$t('stats.rhps')"
          :value="e.rhps | decimal" />
        <hr />
      </article>

      <article class="details-group tank">
        <row
          :title="$t('stats.totalDeaths')"
          :value="e.deaths" />
      </article>
    </section>
  </nav>
</template>

<script>

import { mapState, mapGetters, mapMutations } from 'vuex'

import packageinfo from '@/package.json'
import { filters } from '@/lib/util.js'

import row from '../user/details-row.vue'

export default {
  components: {
    row
  },
  data: () => ({
    dropdown_opened: false,
    version: packageinfo.version,
    releasename: packageinfo.releasename
  }),
  methods: {
    ...mapMutations('ui', [ 'open', 'toggleLayoutMode' ]),
    endEncounter() {
      this.$layer.request('end')
    }
  },
  computed: {
    ...mapState('encounter', {
      e: 'encounter',
      c: 'combatants'
    }),
    ...mapState('settings', [
      'show_decimals',
      'theme'
    ]),
    ...mapGetters('encounter', [ 'rank' ])
  },
  filters: {
    f(value, show_decimals) {
      return filters.decimal(value, Math.min(0, show_decimals))
    }
  }
}

</script>

<style lang="sass">

.c-navbar-wrapper
  @include unselectable
  display: flex
  position: relative

  width: $ui-width
  max-width: 100vw
  height: $nav-height
  margin: 0.25rem auto
  padding: 0
  line-height: $nav-height

  color: $ui-color
  background: $cell-background
  font-size: $nav-text-size
  text-shadow: $shadow-text-background

  @include if-enabled('cell-opaque')
    background-color: adjust-color($cell-background, $alpha: 1)

  @include if-enabled('align-left')
    margin-left: 0

  @include if-enabled('align-right')
    margin-right: 0

  .location
    @include unselectable
    @include overflow-fadeout
    flex-grow: 1
    flex-shrink: 1

    padding: 0 0 0 0.5rem

    overflow: hidden
    word-break: keep-all
    white-space: nowrap
    text-overflow: clip

    time
      background: none
      color: lighten($color-theme, 20%)

      .s::before
        content: ':'

  .info
    @include unselectable
    margin-left: auto

    .rdps::before
      content: '\00a0・\00a0'

  .info, .buttons
    flex-grow: 0
    flex-shrink: 0

  .buttons
    @include clickable
    display: flex
    margin-left: 0.125rem

  .button
    width: $nav-height * 0.75
    height: $nav-height

    filter: drop-shadow($shadow-text-background)
    outline: none
    box-shadow: 0 0 0 -1px red inset

  .dropdown
    position: absolute
    top: $nav-height
    right: 0
    background: $ui-background
    z-index: $z-global-dropdown

    li
      @include clickable
      padding: 0.125rem 0.5rem
      text-align: right

  .c-details
    position: absolute
    top: $nav-height + 0.25rem
    left: 0

    opacity: 0

  .location:hover ~ .c-details
    opacity: 1

  // themes

  @include if-enabled('theme-tokyu', 'theme-keikyu')
    position: relative
    flex-direction: column
    align-items: stretch
    justify-content: center
    text-align: center
    width: 18rem
    height: 4rem
    line-height: 1.25rem
    padding: 0

    .location
      -webkit-mask-image: none
      mask-image: none

    .versioninfo
      display: none

    > .button
      position: absolute
      right: 0
      bottom: 0

    .dropdown, .c-details
      top: 4rem

    .buttons
      position: absolute
      top: 0
      right: 0

  @include if-enabled('theme-tokyu')

    .location
      border-bottom: 0.125rem solid #fff
      padding: 0.25rem 0 0.375rem 0

      time
        display: inline-block
        align-items: center
        color: #fff !important
        background-color: #EE86A7
        width: 2.75rem
        height: 1.25rem
        margin-right: 0.125rem

    .info
      width: 100%
      border-top: 0.5rem solid #EE86A7
      padding-top: 0.125rem

  @include if-enabled('theme-keikyu')
    background: rgba(0, 0, 80, 0.5)

    .location
      width: 100%
      display: flex
      align-items: center
      justify-content: center
      border-bottom: 0.125rem solid skyblue
      flex-grow: 1

      time
        display: flex
        flex-direction: column
        justify-content: center
        line-height: 0.625rem
        margin-right: 0.5rem
        color: #000
        background-color: #fff
        width: 1.75rem
        height: 1.75rem
        border-radius: 50%
        border: 0.125rem solid skyblue

        > span
          display: block
          text-shadow: none !important

        .m
          font-size: 0.625rem

        .s::before
          content: ''

    .info
      width: 100%
      display: flex
      padding: 0 1.5rem
      justify-content: space-between
      border-top: 0.25rem solid white
      height: 1.5rem

      .rdps::before
        display: none

  @include if-enabled('theme-minimal')
    width: $cell-width

    .location
      padding-left: 0.375rem

      time + span
        display: none

    .info
      padding-right: 0.375rem

      .rdps::before
        content: ' '

    .buttons
      margin: 0

      > .button:first-child
        position: absolute
        top: 0
        right: 0
        bottom: 0
        width: 50%
        height: 100%
        opacity: 0

  @include if-enabled('theme-minimal.align-left', 'theme-minimal.align-right')
    margin-top: -1 * $nav-height

  @include if-enabled('theme-korail-tgis')
    background: #00f
    text-shadow: none

    border: 1px solid #fff

    &::before, .location, .info, .dropdown
      font-family: Dotum, 'MS PGothic', sans-serif
      letter-spacing: -0.083em

    .location
      display: flex


      flex-grow: 1
      flex-shrink: 0
      flex-basis: 0
      overflow: hidden

      mask-image: none
      -webkit-mask-image: none

      > time
        color: inherit
        margin-right: 0.5rem

      > span
        flex-grow: 1
        flex-shrink: 0
        flex-basis: 0
        overflow: hidden
        text-align: center
        text-overflow: clip

    .info
      padding-right: 0.5rem

      .rdps::before
        content: '　'

    .location, .info
      position: relative
      padding-left: 2.5rem

      &::before
        display: block
        content: ''

        position: absolute
        top: 0
        bottom: 0
        left: 1.2rem
        margin: auto

        width: 1.5px
        height: 100%

        background: white
        transform: skewX(-56.25deg)
        transform-origin: center

    .buttons
      margin: 0

      > .button
        position: absolute
        right: 0
        width: 4rem
        opacity: 0

    .dropdown
      background: none

      > li
        margin-top: 1px
        border: 1px solid #fff
        background: #00f

        text-align: center

    &::before
      padding: 0 0 0 2.25rem
      content: '운전상태'

      background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAWBAMAAAAoU0G7AAAABlBMVEUAAP////973JksAAAAVUlEQVQoz12PURYAIQgC4QZ4/8uWD3Vt/bAmFBP/0IYIgZH5Rl6iAQV+r2osRfYKFTCd2ux2UMTA5/wA1UMNmJmZuAEP9J/daOGe5VDL2EBrM0sDwgHWxwnAe5IljgAAAABJRU5ErkJggg==')
      background-size: 1.5rem 1.375rem
      background-repeat: no-repeat
      background-position: 0.5rem center

  @include if-enabled('theme-korail-tgis.active')
    &::before
      background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAWBAMAAAAoU0G7AAAABlBMVEUAAP////973JksAAAAUklEQVQoz3WPQQ7AIAgElx8w//9spYBik3IwDg6S1bd8AriMOFfFhQYVZL/sHsLbusDQDzC1ADvaAL1WrVlPF9iBXBtdry86jNhABkqNEWcHdT1GaAh6qM12ZAAAAABJRU5ErkJggg==')

</style>
