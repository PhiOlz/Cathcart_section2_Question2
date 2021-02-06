<template>
  <div style="text-align: center; margin-left:10vw;margin-right:10vw;margin-top:10vh;width:80vw">
      <b-card
        title="Filter Table"
        style="width:100%"
        class="mb-2"
      >
        <b-card-text>
          <label for="input-small">Filter Text:</label>
          <b-form-input id="input-small" type="text" v-model="filterText" style="margin-bottom:20px"></b-form-input>
          <b-table-simple hover small caption-top responsive>
            <b-thead>
              <b-tr>
                <b-th colspan="3">Categories</b-th>
              </b-tr>
            </b-thead>
            <b-tbody>
              <b-tr v-for="(ca_ele, ca_index) in filteredRows.slice((this.currentPage - 1) * this.perPage,this.currentPage * this.perPage,)" :key="`category_`+ca_index">
                <b-td colspan="3">{{ca_ele}}</b-td>
              </b-tr>
            </b-tbody>
          </b-table-simple>
          <b-pagination
						v-model="currentPage"
						:total-rows="filteredRows.length"
						:per-page="perPage"
						aria-controls="card-row"
						align="center"
						></b-pagination>
        </b-card-text>
      </b-card>
    </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    const kat = await $axios.$get('https://api.publicapis.org/categories')
    return { kat }
  },
  data(){
    return {
      categories: [],
      perPage: 3,
      currentPage: 1,
      filterText: '',
    }
  },
  mounted(){
    this.categories = this.kat
  },
  methods:{
    filterTb(){
      this.categories = this.kat
      return this.categories.filter(word => word.toLowerCase().includes(this.filterText.toLowerCase()))
    }
  },
  computed: {
    filteredRows() {
      this.categories = this.kat
      return this.categories.filter(word => word.toLowerCase().includes(this.filterText.toLowerCase()))
    }
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
