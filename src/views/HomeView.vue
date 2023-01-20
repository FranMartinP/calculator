<template>
  <div class="home">
    <CalculatorComponent></CalculatorComponent>
  </div>
</template>

<script>
import CalculatorComponent from '@/components/CalculatorComponent.vue'

export default {
  name: 'HomeView',
  components: {
    CalculatorComponent
  }
}
</script>
