<template>
  <div>
    <h1>Show \{{ item && item['@id'] }}</h1>

    <div
      v-if="isLoading"
      class="alert alert-info"
      role="status">Loading...</div>
    <div
      v-if="error"
      class="alert alert-danger"
      role="alert">
      <span
        class="fa fa-exclamation-triangle"
        aria-hidden="true" /> \{{ error }}
    </div>
    <div
      v-if="deleteError"
      class="alert alert-danger"
      role="alert">
      <span
        class="fa fa-exclamation-triangle"
        aria-hidden="true" /> \{{ deleteError }}
    </div>
    <div
      v-if="item"
      class="table-responsive">
      <table class="table table-striped table-hover">
        <thead>
          <tr>
            <th>Field</th>
            <th>Value</th>
          </tr>
        </thead>
        <tbody>
      {{#each fields}}
          <tr>
            <td>{{name}}</td>
            <td>\{{ item['{{{name}}}'] }}</td>
          </tr>
      {{/each }}
        </tbody>
      </table>
    </div>

    <router-link
      v-if="item"
      :to="{ name: '{{{titleUcFirst}}}List' }"
      class="btn btn-default">Back to list</router-link>
    <button
      class="btn btn-danger"
      @click="deleteItem(item)">Delete</button>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'

export default {
  computed: mapGetters({
    deleteError: '{{{lc}}}/del/error',
    error: '{{{lc}}}/show/error',
    isLoading: '{{{lc}}}/show/isLoading',
    item: '{{{lc}}}/show/retrieved'
  }),

  beforeDestroy () {
    this.reset()
  },

  created () {
    this.retrieve(decodeURIComponent(this.$route.params.id))
  },

  methods: {
    ...mapActions({
      del: '{{{lc}}}/del/del',
      reset: '{{{lc}}}/show/reset',
      retrieve: '{{{lc}}}/show/retrieve'
    }),

    deleteItem (item) {
      if (window.confirm('Are you sure you want to delete this item?')) {
        this.del(item).then(() => this.$router.push({ name: '{{{titleUcFirst}}}List' }))
      }
    }
  }
}
</script>
