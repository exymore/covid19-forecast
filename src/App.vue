<template>
    <div id="app">
        <div class="wrapper">
            <toolbar/>
            <Progress :increasing="3" :stabilized="1"/>
            <Stats/>
            <Select/>
            <fill-data v-show="isStaleData"/>
            <chart :csv-data="csvData" :country="country" v-if="csvData && country"/>
        </div>
        <disclaimer/>
    </div>
</template>

<script>
  import Toolbar from './components/Toolbar';
  import Select from './components/Select';
  import FillData from './components/FillData';
  import Stats from './components/Stats';
  import Disclaimer from './components/Disclaimer';
  import Chart from './components/Chart';
  import Progress from './components/Progress';

  import './assets/App.css';
  import countriesEnum from './enums';
  import axios from 'axios';

  export default {
    name: 'App',
    components: {
      Toolbar,
      Stats,
      Select,
      FillData,
      Disclaimer,
      Chart,
      Progress,
    },

    data: () => ({
      country: null,
      countriesEnum,
      csvData: null,
      isStaleData: false,
    }),

    watch: {
      country() {
        this.fetchCSVData();
      },
    },

    methods: {
      async fetchCSVData() {
        if (this.country) {
          if (countriesEnum.find(country => country === this.country)) {
            const { data } = await Promise.race([
              axios.get(`${process.env.VUE_APP_API_BASE_URI}/forecast?country=${this.country}`),
              axios.get(`${process.env.VUE_APP_STANDBY_API_BASE_URI}/forecast?country=${this.country}`),
            ]);
            this.csvData = [...data];
          } else {
            //TODO: country missing
            console.log('country missing');
          }
        }
      },
    },
  };
</script>

<style scoped>
    .wrapper {
        position: relative;
        top: 100px;
        background-color: white;
        display: flex;
        flex-direction: column;
        margin-left: auto;
        margin-right: auto;
        width: 960px;
        max-width: 960px;
        box-shadow: 0 1px 2px 0 rgba(60, 64, 67, .3), 0 1px 3px 1px rgba(60, 64, 67, .15);
        border-radius: 8px;
        padding: 0 16px;
    }

    @media (max-width: 960px) {
        .wrapper {
            top: 40px;
            width: 480px;
            max-width: 480px;
        }
    }

    @media (max-width: 480px) {
        .wrapper {
            top: 30px;
            width: 360px;
            max-width: 360px;
        }
    }

    @media (max-width: 360px) {
        .wrapper {
            top: 30px;
            width: 300px;
            max-width: 300px;
        }
    }
</style>
