<template>
  <window class="c-changelog-wrapper" :title="$t('changelog.title')" type="changelog">
    <checkbox
      v-model="never_show_changelog_again"
      class="small never-show">
      <span v-show="!never_show_changelog_again">
        {{$t('changelog.onlyAfterUpdates')}}
      </span>
      <span></span>
      {{$t('changelog.never')}}
    </checkbox>
    <hr />
    <version />
    <div style="--display: none; display: block; display: var(--display); font-weight: bold">
      <p style="display: block; padding-left: 1rem">
        If you're seeing this message,
        <br />
        please upgrade to <u>ngld's overlayplugin</u> as fast as possible.
        <br />
        job gauge color will no longer work on older browsers.
      </p>
      <hr />
    </div>
    <group name="0.3.5 'Daikan-yama'" opened>
      <p>
        New jobs support are added.
        <class-colors style="display: flex" />
      </p>
      <p>
        When enable Layout Mode, every detailed view will appear to help resizing.
      </p>
      <p>
        Crits (w/o Directs) removed.
      </p>
      <p>
        …and more forgotten changes.
      </p>
    </group>
    <group name="0.3.4 'Naka-Meguro'">
      <p>
        New option Cell Background added.
        <br />
        (Click to change: <u @click="cell_background = 'translucent'">default</u>
        / <u @click="cell_background = 'opaque'">opaque</u>
        / <u @click="cell_background = 'tinted'">tinted</u>)
      </p>
      <p>
        New theme <u @click="theme = 'korail-tgis'">'Korail TGIS'</u> added.
        <br/>
        <span class="muted">(advices on design is welcome)</span>
      </p>
      <p>
        some colors in kagerou palette has been adjusted.
      </p>
    </group>
    <group name="0.3.3 'Yuutenji'">
      <p>
        Name is alternatively go inside details view (if hidden).
      </p>
      <p>
        Fixed bugs, about overflow on reverse-align, minion overheals,
        overheal percentages, and more.
      </p>
      <p>
        <checkbox
          v-model="force_inline_short_values" >
          Some values now can stay inside singleline cell.
          <span class="muted"> (dps%, oh%, deaths; click to toggle) </span>
        </checkbox>
      </p>
      <p>
        Added 6.0 jobs support.
      </p>
    </group>
    <group name="0.3.2 'Gakugei-Daigaku'">
      <p>
        Align and Sort direction is added to options.
        <br />
        (when non-center align is applied with minimal theme, <br />
        overlay will shown in single line.)
      </p>
    </group>
    <group name="0.3.1 'Jiyugaoka'">
      <p>
        Fixed noisy logs and some bugs (CDH count fix, etc)
      </p>
      <p>
        Job icons are now <u>vector</u>; monochrome also added.
        <br />
        Can be rollbacked to old friendly raster one.
      </p>
      <p>
        DPS % option added.
      </p>
      <p>
        thinner sticks is now configurable.
        <br />
        Now healed stats will be shown default, instead of crits.
      </p>
      <p>
        transfer to Toyoko Line.
      </p>
    </group>
    <group name="0.2.1 'Ookayama'" >
      <p>
        <u @click="toggleLayoutMode()">Layout mode</u>
        added for precise overlay placement.
        <br />
        it's required to resize overlay, as resize handle removed.
      </p>
      <p>
        many fixes about layout, performance, typo etc.
      </p>
      <p>
        Job icon can be shown alone,
        Minimal theme got simpler,
        Swings/GC removed.
      </p>
      <p>
        <u>FFLogs color scheme</u> added. (to be adjusted)
      </p>
      <class-colors class="scheme-fflogs" style="display: flex" />
      <p>
        <i> Upcoming: (auto-)collapse, vertical theme, etc. </i>
      </p>
    </group>
    <group name="0.2.0 'Hatanodai'">
      <p>
        GNB/DNC support.
      </p>
      <p>
        transfer to Ōimachi Line.
      </p>
    </group>
    <group name="0.1.x 'Ikegami Line'">
      <group name="0.1.9 'Nagahara'">
        <p>
          Added Blue Mage support.
        </p>
      </group>
      <group name="0.1.8 'Senzoku-Ike'">
        <p>
          Critical hits and Direct hits was misplaced, so fixed this.
          <br />
          Now, order is
          <span style="color: #1DE9B6">DH</span> -
          <span style="color: #FFCA28">CH</span> -
          <span style="color: #FF6F00">CDH</span>.
        </p>
        <p> removed tailing decimal on RDPS. </p>
        <p> Shorten name is now available. </p>
        <p>
          'Theme' introduced, re-introducing april fool layout.
          <br />
          Preview:
          <u @click="theme = ''">Default</u> /
          <u @click="theme = 'minimal'">Minimal</u> /
          <u @click="theme = 'tokyu'">Tōkyū</u> /
          <u @click="theme = 'keikyu'">Keikyū</u>
        </p>
        <p> Fixed infitite reconnection attempt on 'BeforeLogLineRead' socket. </p>
      </group>
      <group name="0.1.7 'Ishikawa-dai'">
        <p> Click 'Disallow april fool' in submenu to rollback this. </p>
      </group>
      <group name="0.1.6 'Yukigaya-Ootsuka'">
        <p> (this release was updated on this station.) </p>
        <p> trying to hiding resize handle, base on OverlayPlugin settings </p>
      </group>
      <group name="0.1.5 'Ontakesan'">
        <p> (<u>On-ta-ke-san</u>. don't pronounce like <u>on-teik-sein</u>.) </p>
        <p> Critcal count graph also shown on cell (can be disabled) </p>
        <p> Deathcount will not merged from pet. </p>
      </group>
      <group name="0.1.4 'Kugahara'">
        <p> added UI scale adjustment </p>
        <p> color scheme adjusted </p>
        <p> hide name bugfix </p>
      </group>
      <group name="0.1.3 'Chidori-cho'">
        <p> fix Conjurer gauge color </p>
        <p> adjust (may) noticeable margin </p>
        <p> Nameblur added; clicking on name section will toggle it </p>
      </group>
      <group name="0.1.2 'Ikegami'">
        <p> fix 'Hide Job Icons' not working </p>
        <p> added MopiMopi color scheme </p>
      </group>
      <group name="0.1.1 'Hasunuma'">
        <p> Changelog added </p>
        <p> Color scheme can be changed (see Settings) </p>
        <class-colors style="display: flex" />
      </group>
      <group name="0.1.0 'Kamata'">
        <p> Initial commit. </p>
      </group>
    </group>
  </window>
</template>


<script>

import mapStateDynamically from '@/lib/map-state-dynamically.js'

import group from '../settings/group.vue'
import checkbox from '../settings/checkbox.vue'
import window from '../window.vue'
import version from '../version.vue'
import classColors from '../class-colors.vue'

export default {
  components: {
    window,
    group,
    checkbox,
    version,
    classColors
  },
  computed: {
    ...mapStateDynamically('settings', [
      'never_show_changelog_again',
      'theme',
      'force_inline_short_values',
      'cell_background'
    ])
  }
}

</script>

<style lang="sass">

.never-show .label
  display: inline-flex

  span
    margin-right: auto

.changelog

  .c-version p
    display: none

  .c-settings-group-content p
    @include unselectable
    display: block
    margin: 0.5rem 0

  .muted
    opacity: 0.5

</style>
