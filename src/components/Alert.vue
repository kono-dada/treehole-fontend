<template>
  <div>
    <v-alert  show>{{ message }}</v-alert>
  </div>
</template>

<script>
export default {
  props: ['message', 'variant'],
};
</script>
