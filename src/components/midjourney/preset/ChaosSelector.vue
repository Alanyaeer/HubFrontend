<template>
  <div class="field">
    <h2 class="title">{{ $t('midjourney.name.chaos') }}</h2>
    <el-slider v-model="value" :min="0" :max="100" :step="1" class="value" />
    <info-icon :content="$t('midjourney.description.chaos')" class="info" />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import { ElSlider } from 'element-plus';
import InfoIcon from '@/components/common/InfoIcon.vue';

const DEFAULT_CHAOS = 0;

export default defineComponent({
  name: 'ChaosSelector',
  components: {
    ElSlider,
    InfoIcon
  },
  props: {
    modelValue: {
      type: String,
      default: undefined
    }
  },
  emits: ['update:modelValue'],
  data() {
    return {
      value: this.modelValue
    };
  },
  watch: {
    modelValue(val) {
      if (val !== this.value) {
        this.value = val;
      }
    },
    value(val) {
      this.$emit('update:modelValue', val);
    }
  },
  mounted() {
    if (!this.value) {
      this.value = DEFAULT_CHAOS;
    }
    this.$emit('update:modelValue', this.value);
  }
});
</script>

<style lang="scss" scoped>
.field {
  display: flex;
  flex-direction: row;
  align-items: center;

  .title {
    font-size: 14px;
    margin-bottom: 0;
    width: 30%;
  }
  .value {
    flex: 1;
  }
}
</style>
