<template>
    <section v-if="isMobile" class="section">
        <h2 class="subtitle">
            Статистика:
        </h2>
        <b-button @click="hidden = !hidden" type="is-info" class="btn">{{ hidden? 'Показать':'Спрятать' }}</b-button>

        <b-collapse :open="!hidden" animation="slide">
            <div class="group">
                <div class="stats">
                    <Card background-color="#FFEE58" text="Всего заражённых" :value="data.cases"/>
                    <Card background-color="#EF5350" color="#FFFFFF" text="Всего погибших" :value="data.deaths"/>
                    <Card background-color="#8BC34A" text="Всего выздоровевших" :value="data.recovered"/>
                </div>
                <div class="stats">
                    <Card background-color="#FFEE58" text="Заражённых сегодня" :value="data.todayCases"/>
                    <Card background-color="#EF5350" color="#FFFFFF" text="Погибших сегодня" :value="data.todayDeaths"/>
                </div>
            </div>
        </b-collapse>
    </section>
    <div v-else>
        <div class="left">
            <Card background-color="#FFEE58" text="Всего заражённых" :value="data.cases"/>
            <Card background-color="#EF5350" color="#FFFFFF" text="Всего погибших" :value="data.deaths"/>
            <Card background-color="#8BC34A" text="Всего выздоровевших" :value="data.recovered"/>
        </div>
        <div class="right">
            <Card background-color="#FFEE58" text="Заражённых сегодня" :value="data.todayCases"/>
            <Card background-color="#EF5350" color="#FFFFFF" text="Погибших сегодня" :value="data.todayDeaths"/>
        </div>
    </div>
</template>

<script>
  import axios from 'axios';
  import Card from './Card';

  export default {
    name: 'Stats',
    components: {
      Card,
    },
    data: () => ({
      data: {},
      hidden: true,
    }),
    computed: {
      isMobile() {
        return document.documentElement.clientWidth <= 1520;
      },
    },
    async mounted() {
      const { data } = await axios.get('https://corona.lmao.ninja/all');
      this.data = data;
    },
  };
</script>

<style scoped>
    .left {
        position: absolute;
        left: -280px;
        display: flex;
        justify-content: center;
        flex-direction: column;
    }
    .right {
        position: absolute;
        right: -280px;
        display: flex;
        justify-content: center;
        flex-direction: column;
    }
    .section {
        padding-bottom: 0;
    }

    .infected {
        background-color: #FFEE58;
    }

    .dead {
        background-color: #EF5350;
        color: white;
    }

    .recovered {
        background-color: #8BC34A;
    }

    .stats {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }

    .group {
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
        justify-content: center;
    }
</style>
