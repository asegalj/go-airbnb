<template>
  <section class="mobile-country-search">
    <div v-if="activeFilter !== 'mobileCountrySearch'" @click="$emit('setActiveFilter', 'mobileCountrySearch')"
      class="preview-filter flex-box space-between">
      <span class="first">Where</span>
      <span v-if="!country" class="second">Add destination</span>
      <span v-else class="second">{{ country }}</span>
    </div>
    <div v-else class="country-search active">
      <div class="title">Where to?</div>
      <div class="search-mobile">
        <svg viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" role="presentation"
          focusable="false" style="display: block; height: 16px; width: 16px;  color: rgb(34, 34, 34);">
          <path
            d="M13 0c7.18 0 13 5.82 13 13 0 2.868-.929 5.519-2.502 7.669l7.916 7.917-2.828 2.828-7.917-7.916A12.942 12.942 0 0 1 13 26C5.82 26 0 20.18 0 13S5.82 0 13 0zm0 4a9 9 0 1 0 0 18 9 9 0 0 0 0-18z"
            opacity=".8"></path>
        </svg>
        <input v-focus t v-model="country" @input="getCountryList" type="search" placeholder="Search destinations">
      </div>
      <country-modal @setFilterSearch="setFilterSearch" v-if="!isListModalShown"></country-modal>
      <search-list-modal v-else @setInputBySearch="setInputBySearch"></search-list-modal>
    </div>
  </section>
</template>

<script>
import countryModal from './country-modal.vue';
import searchListModal from './search-list-modal.vue';
import { utilService } from '../services/util.service';
export default {
  data() {
    return {
      isListModalShown: false,
      isCountryModalShown: false
    }
  },
  props: {
    activeFilter: String,
    country: String
  },
  mounted() {
    this.getCountryList = utilService.debounce(this.getCountryList)
  },
  methods: {
    setFilterSearch(region) {
      this.$emit('setFilterSearch', region.name)
    },
    getCountryList() {
      this.isListModalShown = (this.country === '') ? false : true
      this.$store.dispatch({ type: 'getCountryList', txt: this.country })
    },
    setInputBySearch(input) {
      this.$emit('setFilterSearch', input)
    }
  },
  components: {
    countryModal,
    searchListModal
  }
}
</script>