<template>
    <JSCharting :options="options" ref="chart" class="columnChart"></JSCharting>
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
            .options({ label: { text: `Максимум заражённых: ${numeral(this.getMaxInfections(this.csvData)).format('0,0')}` } });
        },

        options: {
          type: 'line spline',
          palette: ['crimson', 'rgba(22,125,240,0.49)'],
          legend: {
            template: '%icon %name %max',
            position: 'outside top',
          },
          defaultPoint: {
            tooltip: '%seriesName: <b>%yValue</b>',
            marker: {
              type: 'circle',
              size: 16,
              outline: { color: 'white', width: 1 }
            }
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
                legendEntry_visible: false
              }
            ]
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
    },
  };
</script>

<style scoped>
    .columnChart {
        height: 500px;
    }
</style>
