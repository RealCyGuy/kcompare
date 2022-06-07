<script setup>
defineProps({
    url: String
});
</script>

<template>
  <img :src="url" class="w-1/4 aspect-[274/400] object-cover object-right" />
</template>
