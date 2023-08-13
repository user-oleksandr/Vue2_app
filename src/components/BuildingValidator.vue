<template>
  <div>
    <span :style="{ color: isValidBuilding ? 'green' : 'red' }">
      {{ buildingNumber }}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    buildingNumber: {
      type: String,
      required: true,
    },
  },
  computed: {
    isValidBuilding() {
      return /^[0-9/-]+$/.test(this.buildingNumber);
    },
  },
};
</script>
