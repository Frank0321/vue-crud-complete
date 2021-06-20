<template>
  <liftetimecomponent ></liftetimecomponent>
</template>

<script>
import Liftetimecomponent from "../components/liftetimecomponent";
export default {
  name: "LifetimeEx",
  components: {Liftetimecomponent},
  data () {
    return {
      a: 1
    }
  },
  beforeCreate() {
    console.log('Hook: beforeCreate');
    console.log(` this.a: ${this.a}`);
    console.log(` this.$el: ${this.$el}`);
    console.log();
  },
  created() {
    console.log('Hook: created');
    console.log(` this.a: ${this.a}`);
    console.log(` this.$el: ${this.$el}`);
    console.log();
  },
  beforeMount() {
    console.log('Hook: beforeMount');
    console.log(this.$el.outerHTML);
    console.log();
  },
  mounted() {
    console.log('Hook: mounted');
    console.log(this.$el.outerHTML);
    console.log();
  },
  beforeUpdate() {
    console.log('Hook: beforeUpdate');
    console.log(` this.a: ${this.a}`);
    console.log(` this.$el: ${this.$el}`);
    console.log(this.$el.outerHTML);
    console.log();
  },
  updated() {
    console.log('Hook: updated');
    console.log(` this.a: ${this.a}`);
    console.log(` this.$el: ${this.$el}`);
    console.log(this.$el.outerHTML);
    console.log();
  },
  beforeDestroy() {
    console.log('Hook: beforeDestroy');
    console.log();
  },
  destroyed() {
    console.log('Hook: destroyed');
    console.log();
  }

}

</script>

<style scoped>

</style>