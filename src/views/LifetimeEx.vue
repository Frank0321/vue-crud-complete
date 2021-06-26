<template>
  <div>
    <h1> Hello, This LifeCycle</h1>
    <button @click="updateEvent">updatebtn</button>
    <h2> {{a}}</h2>
  </div>
</template>

<script>
export default {
  name: "LifetimeEx",

  data () {
    return {
      a: 1
    }
  },
  methods: {
    updateEvent: function (){
      this.a += 1;
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
    console.log(this.$el);
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