<template>
  <div class="panel">
    <model-selector v-model="preset.model" class="mb-4" />
    <ratio-selector v-model="preset.ratio" class="mb-4" />
    <quality-selector v-model="preset.quality" class="mb-4" />
    <version-selector v-model="preset.version" class="mb-4" />
    <translation-selector v-model="preset.translation" class="mb-4" />
    <advanced-selector v-model="preset.advanced" class="mb-4" />
    <stylize-selector v-if="preset.advanced" v-model="preset.stylize" class="mb-4" />
    <weird-selector v-if="preset.advanced" v-model="preset.wired" class="mb-4" />
    <chaos-selector v-if="preset.advanced" v-model="preset.chaos" class="mb-4" />
    <image-weight-selector v-if="preset.advanced" v-model="preset.iw" class="mb-4" />
    <niji-selector v-if="false" v-model="preset.niji" class="mb-4" />
    <raw-selector v-if="!preset.model && preset.advanced" v-model="preset.raw" class="mb-4" />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import RatioSelector from './preset/RatioSelector.vue';
import VersionSelector from './preset/VersionSelector.vue';
import StylizeSelector from './preset/StylizeSelector.vue';
import ChaosSelector from './preset/ChaosSelector.vue';
import ModelSelector from './preset/ModelSelector.vue';
import QualitySelector from './preset/QualitySelector.vue';
import ImageWeightSelector from './preset/ImageWeightSelector.vue';
import WeirdSelector from './preset/WeirdSelector.vue';
import RawSelector from './preset/RawSelector.vue';
import AdvancedSelector from './preset/AdvancedSelector.vue';
import TranslationSelector from './preset/TranslationSelector.vue';
import NijiSelector from './preset/NijiSelector.vue';
export default defineComponent({
  name: 'PresetPanel',
  components: {
    ModelSelector,
    QualitySelector,
    RatioSelector,
    VersionSelector,
    StylizeSelector,
    ChaosSelector,
    WeirdSelector,
    RawSelector,
    AdvancedSelector,
    ImageWeightSelector,
    TranslationSelector,
    NijiSelector
  },
  data() {
    return {
      preset: this.$store.state.midjourney.preset
    };
  },
  watch: {
    preset: {
      handler(val) {
        this.$store.dispatch('midjourney/setPreset', {
          ...val
        });
      },
      deep: true,
      immediate: true
    }
  }
});
</script>

<style lang="scss" scoped>
.panel {
  border-right: 1px solid var(--el-border-color);
  height: 100%;
  padding: 10px;
  background-color: var(--el-bg-color);
  .block {
    margin-bottom: 15px;
  }
}
</style>
