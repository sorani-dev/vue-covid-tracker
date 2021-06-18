<template>
  <main v-if="!loading">
    <DataTitle
      :text="title"
      :dataDate="dataDate"
    />

    <DataBoxes :stats="stats" />

    <CountrySelect
      @get-country="getCountryData"
      :countries="countries"
    />

    <button
      @click="clearCountryData"
      v-if="stats.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600 focus:border-black focus:bg-green-600 focus:ring-2 focus:ring-gray-900"
    >Clear Country</button>
  </main>

  <main
    class="flex flex-col align-middle justify-center text-center"
    v-else
  >
    <div class="text-gray-100 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img
      :src="loadingImage"
      class="w-24 m-auto"
      alt=""
    >
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from "@/components/DataTitle.vue";
import DataBoxes from "@/components/DataBoxes.vue";
import CountrySelect from "@/components/CountrySelect.vue";
import { ref } from "@vue/reactivity";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  setup() {
    const loading = ref(true);
    const title = ref("Global");
    const dataDate = ref("");
    const stats = ref({});
    const countries = ref([]);
    const loadingImage = ref(require("../assets/hourglass.gif"));

    const fetchCovidData = async () => {
      try {
        const response = await fetch("https://api.covid19api.com/summary");
        const data = await response.json();
        return data;
      } catch (err) {
        console.error(err);
      }
    };
    /** @param {Object} country */
    const getCountryData = (country) => {
      stats.value = country;
      title.value = country.Country;
    };
    const clearCountryData = async () => {
      loading.value = true;
      const data = await fetchCovidData();
      title.value = "Global";
      stats.value = data.Global;
      loading.value = false;
    };

    const created = async () => {
      const data = await fetchCovidData();

      dataDate.value = data.Date;
      stats.value = data.Global;
      countries.value = data.Countries;
      loading.value = false;
    };
    created();

    return {
      loading,
      title,
      dataDate,
      stats,
      countries,
      loadingImage,
      fetchCovidData,
      getCountryData,
      clearCountryData,
    };
  },
  // data() {
  //   return {
  //     loading: true,
  //     title: "Global",
  //     dataDate: "",
  //     stats: {},
  //     countries: [],
  //     loadingImage: require("../assets/hourglass.gif"),
  //   };
  // },
  // methods: {
  // async fetchCovidData() {
  //   try {
  //     const response = await fetch("https://api.covid19api.com/summary");
  //     const data = await response.json();
  //     return data;
  //   } catch (err) {
  //     console.error(err);
  //   }
  // },
  /** @param {Object} country */
  // getCountryData(country) {
  //   this.stats = country;
  //   this.title = country.Country;
  // },
  // async clearCountryData() {
  //   this.loading = true;
  //   const data = await this.fetchCovidData();
  //   this.title = "Global";
  //   this.stats = data.Global;
  //   this.loading = false;
  // },
  // },
  // async created() {
  //   const data = await this.fetchCovidData();

  //   this.dataDate = data.Date;
  //   this.stats = data.Global;
  //   this.countries = data.Countries;
  //   this.loading = false;
  // },
};
</script>
