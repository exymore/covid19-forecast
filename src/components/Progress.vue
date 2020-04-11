<template>
    <section class="section">
        <h2 class="subtitle">
            Статистика темпов эпидемии
        </h2>
        <div class="progress-wrapper">
            <div class="progress-bar increasing" :style="{width: `${getPercents(increasing)}%`}">
                <h2 class="label-caption-desktop">
                    Растёт в {{this.increasing}} {{this.increasing === 1? 'странe' : 'странах'}}
                </h2>
                <h2 class="label-caption-mobile">
                    ⬆️ {{this.increasing}}
                </h2>
            </div>
            <div class="progress-bar stabilized" :style="{width: `${getPercents(stabilized)}%`}">
                <h2 class="label-caption-desktop">
                    Стабилизировалась в {{this.stabilized}} {{this.stabilized === 1? 'странe' : 'странах'}}
                </h2>
                <h2 class="label-caption-mobile">
                    ➡️ {{this.stabilized}}
                </h2>
            </div>
        </div>
    </section>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'Progress',
    data() {
      return {
        increasing: 0,
        stabilized: 0,
      };
    },
    async mounted() {
      await this.fetchData();
    },
    computed: {
      overall() {
        return this.increasing + this.stabilized;
      },
    },
    methods: {
      async fetchData() {
        const { data } = await Promise.race([
          axios.get(`${process.env.VUE_APP_API_BASE_URI}/stats`),
          axios.get(`${process.env.VUE_APP_STANDBY_API_BASE_URI}/stats`),
        ]);
        this.increasing = data[0].countries_increasing;
        this.stabilized = data[0].countries_stabilized
      },
      getPercents(val) {
        return val / (this.overall / 100);
      },
    },
  };
</script>

<style scoped>
    .label-caption-desktop {
        color: inherit;
        margin-bottom: 0 !important;
        width: 90%;
        word-break: break-word;
        font-size: 1.125rem !important;
        font-weight: 600;
    }

    .label-caption-mobile {
        color: inherit;
        margin-bottom: 0 !important;
        width: auto;
        word-break: break-word;
        font-size: 1.25rem !important;
        font-weight: 600;
        display: none;
    }

    .progress-wrapper {
        margin-top: 16px;
        display: flex;
        width: 100%;
        flex-direction: row;
    }

    .progress-bar {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
        word-break: break-all;
        text-align: center;

        width: 50%;
        height: 60px;
    }

    .increasing {
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
        background-color: #EF5350;
        color: white;
    }

    .stabilized {
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
        background-color: #8BC34A;
        color: white;
    }

    @media (max-width: 960px) {
        .progress-bar {
            height: 50px;
        }

        .label-caption-desktop {
            display: none;
        }

        .label-caption-mobile {
            display: flex;
        }
    }

</style>
