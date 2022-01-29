<template>
  <div class="line-counts">
    <div class="line-count line-count--alt">
      <KotlinLogo class="line-count__icon"/>
      <span class="line-count__name">Lines of Kotlin</span>
      <span class="line-count__value">{{ kotlinLines }}</span>
    </div>
    <div class="line-count">
      <JavaLogo class="line-count__icon"/>
      <span class="line-count__name">Lines of Java</span>
      <span class="line-count__value">{{ javaLines }}</span>
    </div>
    <div class="line-count">
      <JavaLogo class="line-count__icon"/>
      <span class="line-count__name">Original lines of Java</span>
      <span class="line-count__value">{{ originalLines }}</span>
    </div>
    <div class="line-count">
      <JavaLogo class="line-count__icon"/>
      <span class="line-count__name">Java Lines Removed</span>
      <span class="line-count__value">{{ removedLines }}</span>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: 'LineCounts',
  props: {
    kotlinStats: { required: true, type: Object },
    javaStats: { required: true, type: Object },
  },
  data() {
    function addCommas(n: number | string) {
      n = '' + n
      const x = n.split('.');
      let x1 = x[0];
      const x2 = x.length > 1 ? '.' + x[1] : '';
      const rgx = /(\d+)(\d{3})/;
      while (rgx.test(x1)) {
        x1 = x1.replace(rgx, '$1' + ',' + '$2');
      }
      return x1 + x2;
    }

    const originalLines = 272036;
    const removedLines = originalLines - this.javaStats.Code
    return {
      kotlinLines: addCommas(this.kotlinStats.Code),
      javaLines: addCommas(this.javaStats.Code),
      originalLines: addCommas(originalLines),
      removedLines: addCommas(removedLines),
    }
  }
})
</script>
<style scoped lang="scss">

.line-counts {
  flex: auto;
  opacity: 0;
  animation: heading 1s 0.2s ease forwards;
}


.line-count {
  background: #29226c;
  margin: 1rem 0;
  display: flex;
  align-items: center;
  max-width: 500px;

  &--alt {

    .line-count__name {
      color: white;
    }
  }

  &__icon {
    vertical-align: middle;
    margin-right: 1rem;
    background: #2f2780;
    padding: 16px;
    width: 64px;
    height: 64px;
  }

  &__name {
    font-size: 1.5rem;
    color: #666666;
  }

  &__value {
    flex: auto;
    text-align: right;
    font-size: 1.3rem;
    margin-right: 1rem;
    color: #666666;
  }
}


@media all and (max-width: 800px){
  .line-count {
    margin: 1rem auto;
  }

}
@keyframes heading {
  0% {
    transform: translateX(-25%);
    opacity: 0;
  }
  100% {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>
