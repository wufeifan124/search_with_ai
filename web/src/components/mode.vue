<script setup lang="ts">
import { useAppStore } from '../store';
import { SearchMode } from '../contants';
import { ref } from 'vue';
import { useI18n } from 'vue-i18n';
import { TSearchMode } from '../interface';

const { t } = useI18n();

type Emits = {
  (e: 'change', val: TSearchMode): void
}
const emits = defineEmits<Emits>();

const modes = SearchMode.map(item => {
  return {
    name: item.displayName,
    value: item.name,
    disabled: item.disabled
  };
});
const appStore = useAppStore();

const value = ref(appStore.mode);

const onChange = (val: any) => {
  appStore.updateMode(val)
  emits('change', val)
}
</script>

<script lang="ts">
export default {
  name: 'SearchMode'
}
</script>

<template>
  <div class="search-mode">
    <t-radio-group  variant="primary-filled" @change="onChange" :default-value="value">
      <t-radio-button v-for="item in modes" :disabled="item.disabled" :value="item.value">
        {{ t(item.name) }}
      </t-radio-button>
    </t-radio-group>
  </div>
</template>

<style scoped>
.search-mode {
 --td-radius-default: 10px;
 --td-radius-small: 10px;
}
</style>
