<template>
    <section v-if="isMobile" class="section">
        <h2 class="subtitle">
            Мировая статистика:
        </h2>
        <b-button @click="hidden = !hidden" type="is-info" class="btn">{{ hidden? 'Показать':'Спрятать' }}</b-button>

        <b-collapse :open="!hidden" animation="slide">
            <div class="group">
                <div class="stat-section">
                    <h2>
                        Всего:
                    </h2>
                    <div class="stats">
                        <Card v-for="card in statCardsOverall" :key="card.text" :background-color="card.bgColor"
                              :color="card.color"
                              :text="card.text" :value="card.value"/>
                    </div>
                </div>
                <div class="stat-section">
                    <h2>
                        Сегодня:
                    </h2>
                    <div class="stats">
                        <Card v-for="card in statCardsToday" :key="card.text" :background-color="card.bgColor"
                              :color="card.color"
                              :text="card.text" :value="card.value"/>
                    </div>
                </div>
            </div>
        </b-collapse>
    </section>
    <div v-else>
        <div class="left">
            <Card v-for="card in statCardsOverall" :key="card.text" :background-color="card.bgColor" :color="card.color"
                  :text="card.text" :value="card.value"/>
        </div>
        <div class="right">
            <Card v-for="card in statCardsToday" :key="card.text" :background-color="card.bgColor" :color="card.color"
                  :text="card.text" :value="card.value"/>
        </div>
    </div>
</template>

<script>
  import axios from 'axios';
  import numeral from 'numeral';
  import Card from './Card';

  export default {
    name: 'Stats',
    components: {
      Card,
    },
    data: () => ({
      data: {},
      hidden: true,
      intervalId: () => {
      },
    }),
    computed: {
      isMobile() {
        return document.documentElement.clientWidth <= 1520;
      },
      statCardsOverall() {
        return [
          {
            bgColor: '#FFEE58',
            text: 'Всего заражённых',
            value: numeral(this.data.cases).format('0,0'),
          },
          {
            bgColor: '#EF5350',
            color: '#FFFFFF',
            text: 'Всего погибших',
            value: numeral(this.data.deaths).format('0,0'),
          },
          {
            bgColor: '#8BC34A',
            text: 'Всего выздоровевших',
            value: numeral(this.data.recovered).format('0,0'),
          },
        ];
      },
      statCardsToday() {
        return [
          {
            bgColor: '#FFEE58',
            text: 'Заражённых сегодня',
            value: numeral(this.data.todayCases).format('0,0'),
          },
          {
            bgColor: '#EF5350',
            color: '#FFFFFF',
            text: 'Погибших сегодня',
            value: numeral(this.data.todayDeaths).format('0,0'),
          },
        ];
      },
    },
    async mounted() {
      await this.fetchData();

      //Updates data each minute
      this.intervalId = setInterval(async () => {
        await this.fetchData();
      }, 60000);
    },
    methods: {
      async fetchData() {
        const { data } = await axios.get('https://corona.lmao.ninja/all');
        this.data = data;
      },
    },
    destroyed() {
      clearInterval(this.intervalId);
    },
  };
</script>

<style scoped>
    .left {
        position: absolute;
        left: -280px;
        top: -10px;
        display: flex;
        justify-content: center;
        flex-direction: column;
    }

    .right {
        position: absolute;
        right: -280px;
        top: -10px;
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

    .stat-section {
        margin-top: 1rem;
        font-size: 1rem;
    }

    .group {
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
        justify-content: flex-start;
    }

    @media (max-width: 960px) {
        .group {
            justify-content: center;
        }
    }
</style>
