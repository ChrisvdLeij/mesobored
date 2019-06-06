<template>
  <h1>
    {{ activity }}
  </h1>
</template>

<script>
export default {
  name: 'Result',

  props: {
    activity: {
      default: "Push the button",
      type: String
    }
  },

  data() {
    return {
      introText: 'Push the button',
    }
  },
}
</script>
