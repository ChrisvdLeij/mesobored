<template>
<div>
  <div v-if="isLoading">
    Loading
  </div>
  <div v-else>

    <Result :activity="getActivity" />

    <FilterItem v-for="(filter, index) in filters" :filter="filter" @input="onChange" :key="index" />

    <br />
    <br />
    <button class="btn" @click="fetch">Gimme something to do</button>
  </div>
</div>
</template>

<script>
import axios from 'axios';
import Result from './result.vue'
import FilterItem from './filter.vue'

export default {
  name: 'Overview',

  data() {
    return {
      isLoading: false,
      introText: 'Push the button',
      data: {},
      errors: [],
      filters: {
        'alone': {
          'name': 'alone',
          'checked': false,
          'query': 'participants=1',
          'group': 'participants'
        },
        'group': {
          'name': 'group',
          'checked': false,
          'query': 'minparticipants=3',
          'group': 'participants'
        },
        'free': {
          'name': 'free',
          'checked': false,
          'query': 'price=0.0'
        }
      }
    }
  },

  computed: {
    getQuery() {
      let query = '';

      for (const filter in this.filters) {
        if (this.filters[filter].checked) {
          query += `&${this.filters[filter].query}`;
        }
      }

      return query
    },

    getActivity() {
      return this.data.activity
    }
  },

  methods: {
    fetch() {
      this.loading = true;
      axios.get(`http://www.boredapi.com/api/activity?${this.getQuery}`)
        .then(response => {
          this.data = response.data
          this.loading = false;

        })
        .catch(e => {
          this.errors.push(e)
        });
    },

    onChange($event, changeFilter) {
      const status = $event.target.checked;

      // loop over filters and set all radio (group) checked properties too false
      for (const filter in this.filters) {
        if (this.filters[filter].group === changeFilter.group) {
          this.filters[filter].checked = false;
        }
      }

      // set the status of the change input
      changeFilter.checked = status;
    }
  },

  components: {
    Result,
    FilterItem
  }
}
</script>
