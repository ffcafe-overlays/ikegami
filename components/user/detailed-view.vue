<template>
  <section class="c-details">
    <article class="details-group" v-if="hide_name">
      <row
        title=""
        :value="c.name" />
      <hr />
    </article>

    <article class="details-group dps">
      <row
        :title="$t('stats.dps')"
        :value="c.dps | decimal" />
      <row
        :title="$t('stats.1m')"
        :value="c.dps1m | decimal" />
      <row
        :title="$t('stats.maxhit')"
        :value="c.maxhit" />

      <hr v-if="!reduced" />

      <row
        :title="$t('stats.swings')"
        :value="c.swings"
        v-if="!reduced" />
      <!-- <row
        title="Swings/GC"
        :value="c.swings / e.duration * 2.5 | decimal" /> -->
      <row
        title="D/!/!!!"
        :value="c.critcounts.join('/')"
        v-if="!reduced" />
      <div class="row">
        <graph type="dps-crit" :combatant="c" />
      </div>
      <hr />
    </article>

    <article class="details-group healer" v-if="c.healed">
      <row
        :title="$t('stats.hps')"
        :value="c.hps | decimal" />
      <row
        :title="$t('stats.pct')"
        :value="(c.healed / e.healed) | pct" />
      <row
        :title="$t('stats.maxheal')"
        :value="c.maxheal" v-if="!reduced" />

      <hr v-if="!reduced" />

      <row
        :title="$t('stats.overheal')"
        :value="(c.oh / c.healed) | pct" />
      <row
        :title="$t('stats.minionHeal')"
        :value="(c.minion_heal / c.healed) | pct"
        v-if="!reduced && c.minion_heal" />
      <row
        :title="$t('stats.shield')"
        :value="c.shield"
        v-if="!reduced" />
      <div class="row">
        <graph type="healer-pct" :combatant="c" />
      </div>

      <hr />
    </article>

    <article class="details-group tank">
      <row
        :title="$t('stats.deaths')"
        :value="c.deaths" />
    </article>
  </section>
</template>

<script>

import { mapState } from 'vuex'

import row from '../user/details-row.vue'
import graph from '../user/graph.vue'

export default {
  components: {
    row,
    graph
  },
  props: {
    c: Object, // combatant
    e: Object // encounter
  },
  computed: {
    ...mapState('settings', [
      'hide_name',
      'reduced'
    ])
  }
}

</script>

<style lang="sass">

.c-details
  @include unselectable
  display: flex
  flex-direction: column

  width: $details-width

  margin-left: ($details-width - $cell-width) * -0.5
  padding: 0.375rem

  background-color: $details-background
  color: #fff

  overflow-x: auto
  transition: opacity 200ms ease

  pointer-events: none
  z-index: $z-details

  hr
    margin: 0.25rem 0
    border: solid $details-seperator
    border-width: 0 0 $_1px 0

  var
    font-style: normal

  .row
    border-left: 0.125rem solid #fff8
    padding-left: 0.25rem

  .c-details-graph
    padding: 0.25rem 0
    height: 0.75rem

.details-group
  &.dps .row
    border-left-color: #FF6E2C
  &.healer .row
    border-left-color: #00E676
  &.tank .row
    border-left-color: #82B1FF

</style>
