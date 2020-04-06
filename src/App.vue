<template>
    <div id="app">
        <div class="wrapper">
            <toolbar/>
            <Stats/>
            <Select/>
            <fill-data v-if="isStaleData"/>
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
  import './assets/App.css';

  export default {
    name: 'App',
    components: {
      Toolbar,
      Stats,
      Select,
      FillData,
      Disclaimer,
    },

    data: () => ({
      country: null,
      csvData: null,
      isStaleData: true,
    }),

    methods: {},
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
