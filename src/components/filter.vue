<template>
<div style="display: inline-flex; margin: 0 10px">
  <input :type="getType" :name="getGroup" :value="getName" @input="onChange($event, filter)" />
  <label style="margin-left:5px;"> {{ getName }}</label>
</div>
</template>

<script>
export default {
  name: 'FilterItem',

  props: {
    filter: {
      required: false,
      type: Object
    }
  },

  computed: {
    getType() {
      if (this.filter.group) return 'radio';

      return 'checkbox';
    },

    getName() {
      return this.filter.name
    },

    getGroup() {
      return this.filter.group
    }
  },

  methods: {
    onChange($event, filter) {
      this.$emit('input', $event, filter);
    }
  }
}
</script>
