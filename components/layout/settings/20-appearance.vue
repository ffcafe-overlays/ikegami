<template>
  <div class="tab-content">
    <!-- 62.5 = 10px = usual zoomout limit -->
    <number
      :label="$t('settings.appearance.uiScale')"
      :min="62.5"
      :max="300"
      :step="6.25"
      :default="100"
      v-model="ui_scale" />
    <dropdown
      :label="$t('settings.appearance.theme')"
      :empty="$t('settings.appearance.themeDefault')"
      :selections="new Map([
        [ 'minimal', 'Minimal' ],
        [ 'korail-tgis', 'Korail TGIS' ],
        [ 'tokyu', 'Tōkyū Ikegami Line' ],
        [ 'keikyu', 'Keikyū Line' ]
      ])"
      v-model="theme" />
    <dropdown
      label="Cell Background"
      :selections="new Map([
        [ 'translucent', 'Translucent' ],
        [ 'opaque', 'Opaque' ],
        [ 'tinted', 'Tinted' ]
      ])"
      v-model="cell_background" />
    <dropdown
      label="Color Scheme"
      :selections="new Map([
        [ 'kagerou-material', $t('settings.appearance.colorSchemeKagerouMaterial') ],
        [ 'mopimopi-default', $t('settings.appearance.colorSchemeMopimopiDefault') ],
        [ 'amethyst', $t('settings.appearance.colorSchemeAmethyst') ],
        [ 'fflogs', $t('settings.appearance.colorSchemeFflogs') ],
        [ 'ikegami-by-role-3', $t('settings.appearance.colorSchemeIkegamiByRole3') ],
        [ 'ikegami-by-role-5', $t('settings.appearance.colorSchemeIkegamiByRole5') ],
        [ 'mopimopi-by-role', $t('settings.appearance.colorSchemeMopimopiByRole') ]
      ])"
      v-model="color_scheme" />
    <dropdown
      :label="$t('settings.appearance.jobIcons')"
      :selections="new Map([
        [ 'default', $t('settings.appearance.jobIconsDefault') ],
        [ 'monochrome', $t('settings.appearance.jobIconsMonochrome') ],
        [ 'legacy', $t('settings.appearance.jobIconsLegacy') ]
      ])"
      v-model="job_icons" />
    <class-colors />
    <checkbox
      :label="$t('settings.appearance.blurName')"
      v-model="blur_name" />
    <blockquote>
      {{ $t('settings.appearance.blurNameTip') }}
    </blockquote>
  </div>
</template>

<script>

import mapStateDynamically from '@/lib/map-state-dynamically.js'

import * as components from './_components.js'

export default {
  components,
  computed: {
    ...mapStateDynamically('settings', [
      'ui_scale',
      'theme',
      'cell_background',
      'color_scheme',
      'job_icons',
      'blur_name'
    ])
  }
}

</script>
