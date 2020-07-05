<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12" lg="6">
        <base-material-card
          id="multiple-bar"
          color="primary"
          icon="mdi-shape-outline"
          class="px-4 py-3"
        >
          <template v-slot:after-heading>
            <div class="display-1 mt-2 font-weight-light">Categories Founding Performance</div>
          </template>
          <chartist
            :data="foundingPerformanceBar.data"
            :options="foundingPerformanceBar.options"
            type="Bar"
            style="max-height: 1000px;"
            class="mt-3"
          />
        </base-material-card>
        <div class="py-3" />
      </v-col>
      <v-col cols="12" lg="6">
        <base-material-card
          id="backers-bar"
          color="primary"
          icon="mdi-account-supervisor-outline"
          class="px-4 py-3"
        >
          <template v-slot:after-heading>
            <div class="display-1 mt-2 font-weight-light">Backers by Category</div>
          </template>
          <chartist
            :data="backersBar.data"
            :options="backersBar.options"
            type="Bar"
            style="max-height: 1000px;"
            class="mt-3"
          />
        </base-material-card>
        <div class="py-3" />
      </v-col>
      <v-col cols="12" lg="6">
        <base-material-card
          id="project-performance"
          color="primary"
          icon="mdi-traffic-light"
          title="Projects Performance by Status"
          class="px-4 py-3"
        >
          <chartist
            :data="projectPerformance.data"
            :options="projectPerformance.options"
            type="Pie"
            style="max-height: 250px;"
          />

          <v-divider class="ma-3" />

          <div class="px-3">
            <div class="body-2 text-uppercase grey--text font-weight-bold mb-3">Project Status</div>

            <v-row align="center" class="ma-0">
              <v-avatar class="mr-1" color="info" size="12" />

              <span class="mr-3 font-weight-light">Successful</span>

              <v-avatar class="mr-1" color="warning" size="12" />

              <span class="mr-3 font-weight-light">Failed</span>

              <v-avatar class="mr-1" color="red" size="12" />

              <span class="mr-3 font-weight-light">Canceled</span>
              <v-avatar class="mr-1" color="green" size="12" />

              <span class="mr-3 font-weight-light">Suspended</span>
            </v-row>
          </div>
        </base-material-card>
      </v-col>
      <v-col cols="12" lg="6">
        <base-material-card
          id="usd-pledged-performance"
          color="primary"
          icon="mdi-cash-multiple"
          title="USD Pledged Performance by Status"
          class="px-4 py-3"
        >
          <chartist
            :data="usdPledgedPerformance.data"
            :options="usdPledgedPerformance.options"
            type="Pie"
            style="max-height: 250px;"
          />

          <v-divider class="ma-3" />

          <div class="px-3">
            <div class="body-2 text-uppercase grey--text font-weight-bold mb-3">Legend</div>

            <v-row align="center" class="ma-0">
              <v-avatar class="mr-1" color="info" size="12" />

              <span class="mr-3 font-weight-light">Successful</span>

              <v-avatar class="mr-1" color="warning" size="12" />

              <span class="mr-3 font-weight-light">Failed</span>

              <v-avatar class="mr-1" color="red" size="12" />

              <span class="mr-3 font-weight-light">Canceled</span>
              <v-avatar class="mr-1" color="green" size="12" />

              <span class="mr-3 font-weight-light">Suspended</span>
            </v-row>
          </div>
        </base-material-card>
      </v-col>
      <v-col cols="12">
        <base-material-card icon="mdi-earth" title="Projects Distribution by Country">
          <v-row>
            <v-col cols="12" md="6" class="mt-10">
              <v-simple-table class="ml-2">
                <thead>
                  <tr>
                    <th class="primary--text">Country</th>
                    <th class="primary--text"># Projects</th>
                    <th class="primary--text">Percentage</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(sale, i) in sales" :key="i">
                    <td v-text="sale.country" />
                    <td v-text="sale.salesInM" />
                    <td v-text="((sale.salesInM / totalSales) * 100).toFixed(2) + '%'" />
                  </tr>
                </tbody>
              </v-simple-table>
            </v-col>

            <v-col cols="12" md="6">
              <v-world-map :country-data="countryData" high-color="#5757ff" low-color="#aeaefd" />
            </v-col>
          </v-row>
        </base-material-card>
      </v-col>
      <v-col cols="12">
        <base-material-card
          id="correlation-line"
          color="primary"
          icon="mdi-alarm-multiple"
          class="px-4 py-3"
        >
          <template v-slot:after-heading>
            <div
              class="display-1 mt-2 font-weight-light"
            >Correlation b/w Launch Month & Pledged Amount</div>
          </template>
          <chartist
            :data="correlationChart.data"
            :options="correlationChart.options"
            type="Line"
            style="max-height: 225px;"
            class="mt-3"
          />
        </base-material-card>
        <div class="py-3" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      foundingPerformanceBar: {
        data: {
          labels: [
            "Games",
            "Design",
            "Technology",
            "Film & Video",
            "Music",
            "Publishing",
            "Fashion",
            "Food",
            "Art",
            "Comics",
            "Theater",
            "Photography",
            "Crafts",
            "Dance",
            "Journalism"
          ],
          series: [
            [
              739812939,
              728596083,
              681160267,
              389394755,
              192883933,
              132765395,
              129776243,
              125162762,
              90216499,
              71337170,
              43584923,
              38276943,
              14324893,
              12997814,
              12341613
            ],
            [
              1576903747,
              1251416536,
              3303019009,
              5140831078,
              778982759,
              983021180,
              506496753,
              1202023731,
              1100517820,
              211379314,
              295119116,
              131747084,
              91273717,
              35931653,
              408502063
            ]
          ]
        },
        options: {
          lineSmooth: this.$chartist.Interpolation.none(),
          stretch: true,
          seriesBarDistance: 10,
          reverseData: true,
          horizontalBars: true,
          axisX: {
            scaleMinSpace: 100,
            labelInterpolationFnc: function(value) {
              return `${Math.abs(value / 1000000)}M`;
            }
          },
          axisY: {
            showGrid: false
          },
          chartPadding: {
            top: 0,
            right: 50,
            bottom: 10,
            left: 50
          }
        }
      },
      backersBar: {
        data: {
          labels: [
            "Games",
            "Design",
            "Technology",
            "Film & Video",
            "Music",
            "Publishing",
            "Comics",
            "Fashion",
            "Food",
            "Art",
            "Theater",
            "Photography",
            "Crafts",
            "Journalism",
            "Dance"
          ],
          series: [
            11311808,
            7188108,
            5311010,
            4191962,
            2701264,
            2218584,
            1454046,
            1395568,
            1327368,
            1181924,
            512420,
            426466,
            239774,
            181218,
            161070
          ]
        },
        options: {
          distributeSeries: true,
          lineSmooth: this.$chartist.Interpolation.none(),
          stretch: true,
          seriesBarDistance: 10,
          axisY: {
            labelInterpolationFnc: function(value) {
              return `${Math.abs(value / 1000000)}M`;
            }
          },
          axisX: {
            offset: 60,
            showGrid: false
          },
          chartPadding: {
            top: 0,
            right: 50,
            bottom: 10,
            left: 50
          }
        }
      },
      projectPerformance: {
        data: {
          series: [36, 53.1, 10.4, 0.5]
        },
        options: {
          labelOffset: 65,
          chartPadding: 30,
          labelInterpolationFnc: value => `${value}%`
        },
        responsiveOptions: [
          [
            "screen and (min-width: 640px)",
            {
              chartPadding: 30,
              labelOffset: 100,
              labelDirection: "explode",
              labelInterpolationFnc: function(value) {
                return value;
              }
            }
          ],
          [
            "screen and (min-width: 1024px)",
            {
              labelOffset: 80,
              chartPadding: 20
            }
          ]
        ]
      },
      usdPledgedPerformance: {
        data: {
          series: [89.2, 7.7, 2.7, 0.5]
        },
        options: {
          labelOffset: 0,
          chartPadding: 0,
          labelInterpolationFnc: value => `${value}%`
        },
        responsiveOptions: [
          [
            "screen and (min-width: 640px)",
            {
              chartPadding: 30,
              labelOffset: 100,
              labelDirection: "explode",
              labelInterpolationFnc: function(value) {
                return value;
              }
            }
          ],
          [
            "screen and (min-width: 1024px)",
            {
              labelOffset: 80,
              chartPadding: 20
            }
          ]
        ]
      },
      sales: [
        {
          country: "USA",
          salesInM: 290238
        },
        {
          country: "Great Britain",
          salesInM: 1300
        },
        {
          country: "Australia",
          salesInM: 7442
        },
        {
          country: "Germany",
          salesInM: 3721
        },
        {
          country: "France",
          salesInM: 3721
        },
        {
          country: "Italy",
          salesInM: 3721
        },
        {
          country: "Spain",
          salesInM: 3721
        }
      ],
      countryData: {
        US: 290238,
        GB: 33489,
        AU: 7442,
        DE: 3721,
        FR: 3721,
        IT: 3721,
        ES: 3721
      },
      correlationChart: {
        data: {
          labels: [
            "Jul",
            "Mar",
            "Oct",
            "May",
            "Jun",
            "Nov",
            "Aug",
            "Apr",
            "Sep",
            "Feb",
            "Jan",
            "Dec"
          ],
          series: [
            [
              0.097015226,
              0.090067329,
              0.089164236,
              0.087766594,
              0.087121528,
              0.08606792,
              0.086006101,
              0.085592184,
              0.082694763,
              0.078859308,
              0.073559017,
              0.056085794
            ],
            [
              0.094938896,
              0.093884984,
              0.09673002,
              0.099228986,
              0.087919002,
              0.09478709,
              0.073419653,
              0.086812185,
              0.094308442,
              0.078854731,
              0.061706675,
              0.037409337
            ]
          ]
        },
        options: {
          low: 0,
          high: 0.1,
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 5
          }
        }
      }
    };
  },
  methods: {
    complete(index) {
      this.list[index] = !this.list[index];
    }
  },
  computed: {
    totalSales() {
      return this.sales.reduce((acc, val) => acc + val.salesInM, 0);
    }
  }
};
</script>

<style lang="sass">
#multiple-bar
  .ct-series-a .ct-bar
    stroke: #5757ff !important

    .ct-series-b .ct-bar
      stroke: #5757ff !important

#backers-bar
  .ct-chart-bar .svg.ct-chart-line
    overflow: visible

  .ct-label.ct-label.ct-horizontal.ct-end
    position: relative
    justify-content: flex-end
    text-align: right
    transform-origin: 100% 0
    transform: translate(-100%) rotate(-45deg)
    white-space: nowrap

  .ct-series-a .ct-bar
    stroke: #5757ff !important

    .ct-series-b .ct-bar
      stroke: #5757ff !important
  
#project-performance
  .ct-series-a .ct-slice-pie
    fill: #00cae3 !important

  .ct-series-b .ct-slice-pie
    fill: #fb8c00 !important

  .ct-series-c .ct-slice-pie
    fill: #f44336 !important

  .ct-series-d .ct-slice-pie
    fill: #4caf50 !important

#usd-pledged-performance
  .ct-series-a .ct-slice-pie
    fill: #00cae3 !important

  .ct-series-b .ct-slice-pie
    fill: #fb8c00 !important

  .ct-series-c .ct-slice-pie
    fill: #f44336 !important

  .ct-series-d .ct-slice-pie
    fill: #4caf50 !important

#correlation-line
  .ct-series-a .ct-line,
  .ct-series-a .ct-point
    stroke: #00cae3 !important
  #multiple-bar
    .ct-series-a .ct-bar
      stroke: #5757ff !important
    .ct-series-b .ct-bar
      stroke: #f44336 !important
  
</style>
