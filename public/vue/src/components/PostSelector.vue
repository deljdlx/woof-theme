<template>
    <div>

      <v-autocomplete
        dense
        clearable
        :items="posts"
        item-text="title.rendered"
        item-value="id"

        :loading="loading"
        cache-items
        hide-no-data
        hide-details
        filled


        v-model="value"

        :search-input.sync="search"
        @change="update"
        @click:clear="update"
      ></v-autocomplete>

    </div>
</template>

<script>

export default {

  name: 'PostSelector',

  created() {

    this.value = this.customizerData.data.value;
    this.apiURL = this.customizerData.data.apiURL;
    this.postType = this.customizerData.data.postType;

    if(this.value) {
      this.$axios.get(
        this.apiURL + '/' + this.postType + '/' + this.value +'?_embed=1'
      ).then(response => {
        this.posts = [response.data];
      });
    }

    this.querySelections('');

  },

  props: [
    'customizerData'
  ],


  data() {
    return {
      postType: null,
      apiURL: null,
      value: null,
      search: null,
      loading: false,
      posts: []
    };
  },


  watch: {
    search (search) {

      search && search !== this.select && this.querySelections(search)
      // this.querySelections(search);
      // val && val !== this.select && this.querySelections(val)
    },
  },


  methods: {
    querySelections (search) {
      this.loading = true;
      this.$axios.get(
        this.apiURL +  '/' + this.postType + '?_embed=1&search=' + search
      ).then(response => {
        this.posts = response.data;

        this.loading = false;
      });


      // this.loading = true
      // Simulated ajax query

      /*
      setTimeout(() => {
        this.items = this.states.filter(e => {
          return (e || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
        })
        this.loading = false
      }, 500)
      */
    },


    update(value) {
      this.customizerData.control.setting.set(value);
    }
  }



};
</script>
