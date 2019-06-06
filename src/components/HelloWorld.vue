<template>
<div class="hello">
  <div v-if="isLoading">
    Loading
  </div>
  <div v-else>

    <h1>
      {{ getactivity }}
    </h1>

    <!-- http://www.boredapi.com/api/activity?{{getAloneQuery}}&{{getFreeQuery}}&{{getGroupQuery}} -->

    <div v-for="(filter, index) in filters" :key="index" style="display: inline-flex">
      <input :type="typeFilter(filter)" :name="filter.group" :value="filter.name" @input="onChange($event, filter)" /> <label style="margin: 0 10px">{{filter.name}}</label>
    </div>
    <br />
    <br />
    <button class="btn" @click="fetch">Gimme something to do</button>

    <div v-if="errors.length">
      {{ errors }}
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',

  data() {
    return {
      isLoading: false,
      introText: 'Push the button',
      data: [],
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
    getactivity() {
      if (this.data.activity) {
        return this.data.activity
      }

      return this.introText;
    },

    getFreeQuery() {
      if (this.filters.free.checked) {
        return this.filters.free.query;
      }

      return '';
    },

    getGroupQuery() {
      if (this.filters.group.checked) {
        return this.filters.group.query;
      }

      return '';
    },

    getAloneQuery() {
      if (this.filters.alone.checked) {
        return this.filters.alone.query;
      }

      return '';
    }
  },

  methods: {
    fetch() {
      this.loading = true;
      axios.get(`http://www.boredapi.com/api/activity?${this.getAloneQuery}&${this.getFreeQuery}&${this.getGroupQuery}`)
        .then(response => {
          this.data = response.data
          this.loading = false;

        })
        .catch(e => {
          this.errors.push(e)
        });
    },

    onChange($event, clickedFilter) {
      const status = $event.target.checked;

      // loop over filters and set all checked properties too false
      for (const filter in this.filters) {
        if(this.filters[filter].group === clickedFilter.group){
            this.filters[filter].checked = false;
        }
      }

      // set the status of the change input
      clickedFilter.checked = status;
    },

    typeFilter(filter) {
      if (filter.group) {
        return 'radio'
      }
      return 'checkbox'
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
