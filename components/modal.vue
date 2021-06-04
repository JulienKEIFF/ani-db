<template>
  <div class="absolute top-0 left-0 bg-red-500 z-50 p-16">
      suprimer cet {{name}}
  </div>
</template>

<script>
export default {
    name: "modal",
    props: ['name'],
    methods: {
        validate: function() {
            this.$emit('validate');
        },
        cancel: function() {
            this.$emit('cancel');
        }
    }
}
</script>