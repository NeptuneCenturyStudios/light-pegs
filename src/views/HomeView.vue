<template>
  <div class="d-flex">
    <div class="ml-auto mr-auto mt-5">
    <PegBoard></PegBoard>
  </div>
  </div>
</template>

<script>
import PegBoard from '@/components/PegBoard.vue';

export default {
  name: 'HomeView',
  components: {
    PegBoard
  }
}
</script>
