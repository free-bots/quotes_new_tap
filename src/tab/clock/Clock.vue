<template>
  <div v-if="showSec">
    <h1>
      {{ hour.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}
      <Dot class="dot" />
      {{ min.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}
      <Dot class="dot" />
      {{ sec.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}
    </h1>
  </div>
  <div v-else>
    <h1>
      {{ hour.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}
      <Dot class="dot" />
      {{ min.toLocaleString(undefined, { minimumIntegerDigits: 2 }) }}
    </h1>
  </div>
</template>

<script>
import Dot from "../dot/Dot";
export default {
  name: "clock",
  props: ["showSec", "hour", "min", "sec"],
  components: {
    Dot
  }
};
</script>

<style scoped>
h1 {
  font-size: 15vw;
  text-align: center;
  margin: 0;
}
.dot {
  position: relative;
  top: -4vw;
}
</style>
