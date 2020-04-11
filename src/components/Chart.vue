<template>
    <div>
        <div class="div-section">
            <div :class="`placeholder ${isGrowthForCountryStabilized? 'stabilized' : 'increasing'}`">
                <h1 class="caption">Рост количества заражённых {{isGrowthForCountryStabilized? 'стабилизировался' :
                    'увеличивается'}}</h1>
                <h1 class="caption">На сегодняшний день инфицировано {{getTodayInfectionsString()}} человек</h1>
                <h1 class="caption">Ожидается
                    {{`${getMaxInfectionsString()} ${getMaxInfectionsString().slice(-1) === '1' ?
                    'заражённый' : 'заражённых'}`}} к {{lastDate}}</h1>
            </div>
            <JSCharting :options="options" ref="chart" class="columnChart"></JSCharting>
        </div>
    </div>
</template>

<script>
  import JSCharting from 'jscharting-vue';
  import numeral from 'numeral';

  export default {
    name: 'Chart',
    components: {
      JSCharting,
    },
    props: {
      country: {
        type: String,
        default: '',
      },
      csvData: {
        type: Array,
        default: () => [],
      },
      isStabilized: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        updateData: () => {
          const chart = this.$refs.chart.instance;
          this.options = {
            ...this.options,
            series: [
              {
                options: { emptyPointMode: 'default' },
                name: 'Инфицировано',
                points: this.getActualData(),
              },
              {
                name: 'Ожидается',
                points: this.getForecastData(),
              },
            ],
          };
          chart
            .axes('y')
            .markers(0)
            .options({ value: this.getMaxInfections(this.csvData) });
          chart
            .axes('y')
            .markers(0)
            .options({ label: { text: `Максимум заражённых: <b>${numeral(this.getMaxInfections(this.csvData)).format('0,0')}</b>` } });
        },

        options: {
          type: 'line spline',
          palette: ['crimson', 'rgba(22,125,240,0.49)'],
          legend: {
            template: '%icon %name',
            position: 'outside top',
          },
          defaultPoint: {
            tooltip: '%seriesName: <b>%yValue</b>',
            marker: {
              type: 'circle',
              size: 16,
              outline: { color: 'white', width: 1 },
            },
          },
          series: [
            {
              options: { emptyPointMode: 'default' },
              name: 'Инфицировано',
              points: this.getActualData(),
            },
            {
              name: 'Ожидается',
              points: this.getForecastData(),
            },
          ],
          yAxis: [
            {
              markers: [
                {
                  legendEntry_visible: false,
                  value: this.getMaxInfections(this.csvData),
                  label: {
                    text:
                      `Максимум заражённых: <b>${numeral(this.getMaxInfections(this.csvData)).format('0,0')}</b>`,
                    style_fontSize: 12,
                    align: 'center',
                  },
                  color: ['#000290', 1],
                },
              ],
            },
          ],
          xAxis: {
            cultureName: 'ru',
            crosshair_enabled: true,
            formatString: 'M',
            scale: {
              type: 'time',
            },
            markers: [
              {
                value: new Date().toLocaleString().split(',')[0],
                legendEntry_visible: false,
              },
            ],
          },
          toolbar_visible: false,
        },
      };
    },
    watch: {
      csvData: {
        handler() {
          this.updateData();
        },
      },
    },
    computed: {
      isGrowthForCountryStabilized() {
        const currentDate = this.csvData[0].current_date;
        return this.csvData.find(day => day.ds === currentDate).growth_stabilized;
      },
      lastDate() {
        return new Date(this.csvData[this.csvData.length - 1].ds).toLocaleDateString('ru-RU', {
          year: 'numeric',
          month: 'long',
          day: 'numeric',
        });
      },
    },
    methods: {
      getActualData() {
        return this.csvData.filter(el => el.y > 0).map(el => ({
            name: el.ds,
            y: Math.round(el.y),
            marker: {
              size: 6,
            },
          }
        ));
      },
      getForecastData() {
        return this.csvData.filter(el => el.y !== 0).map(el => ({
            name: el.ds,
            y: Math.round(el.yhat_upper),
            marker: {
              size: 6,
            },
          }
        ));
      },
      getMaxInfections(val) {
        return Math.round(Math.max.apply(null, val.map(x => x.yhat_upper)));
      },
      getMaxInfectionsString() {
        return numeral(this.getMaxInfections(this.csvData)).format('0,0');
      },
      getTodayInfectionsString() {
        const filtered = this.csvData.filter(el => el.y);
        return numeral(filtered[filtered.length - 1].y).format('0,0');
      },
    },
  };
</script>

<style scoped>
    .div-section {
        padding: 0 1.5rem 2rem;
    }

    .placeholder {
        padding: 1rem 1rem;
        margin-bottom: 1rem;
        border-radius: 8px;
        width: auto;
    }

    .columnChart {
        height: 500px;
    }

    .caption {
        color: #7924d4;
        font-size: 1.25rem;
        font-weight: 600;
        margin-bottom: 4px;
    }

    @media (max-width: 960px) {
        .caption {
            color: #7924d4;
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .div-section {
            padding: 0 !important;
        }

        .caption {
            font-size: 1rem;
        }
    }
</style>
