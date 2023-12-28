<template>
  <router-view v-slot="{ Component, route }">
    <keep-alive>
      <component
        :is="Component"
        v-if="$route.meta.keepAlive"
        :key="route.name"
      />
    </keep-alive>
    <component
      :is="Component"
      v-if="!$route.meta.keepAlive"
      :key="route.name"
    />
  </router-view>
</template>

<script lang="ts" setup>
import { ref, watch } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();

const includeList = ref<string[]>([])


watch(() => route, (newVal) => {
  if (newVal.meta.keepAlive && includeList.value.indexOf(newVal.name as never) === -1) {
    includeList.value.push(newVal.name as never);
  }
}, { deep: true });

</script>
