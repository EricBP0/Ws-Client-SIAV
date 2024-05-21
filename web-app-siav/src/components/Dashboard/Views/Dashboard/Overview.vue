<template>
  <div>
    <div class="row">
      <div class="col-lg-3 col-md-6 col-sm-6" v-for="stats in statsCards">
        <stats-card :type="stats.type"
                    :icon="stats.icon"
                    :small-title="stats.title"
                    :title="stats.value">
          <div class="stats" slot="footer">
            <i :class="stats.footerIcon"></i>
            {{stats.footerText}}
          </div>
        </stats-card>
      </div>
    </div>

    <div class="row">
        <div class="col-lg-12">
          <div class="card">
            <div class="card-header">
              <h4 class="card-title">Regiões Disponíveis</h4>
            </div>
            <div class="card-body">
              <div class="row">
                <div class="col-md-7">
                  <world-map></world-map>
                </div>
                <div class="col-md-5">
                  <div class="table-responsive">
                    <table class="table">
                      <tbody>
                      <tr>
                        <td>
                          <div class="flag">
                            <img src="static/img/flags/US.png" alt="US">
                          </div>
                        </td>
                        <td>USA</td>
                        <td class="text-right">
                          2.920
                        </td>
                      </tr>
                      <tr>
                        <td>
                          <div class="flag">
                            <img src="static/img/flags/DE.png" alt="DE">
                          </div>
                        </td>
                        <td>Germany</td>
                        <td class="text-right">
                          1.300
                        </td>
                      </tr>
                      <tr>
                        <td>
                          <div class="flag">
                            <img src="static/img/flags/AU.png" alt="AU">
                          </div>
                        </td>
                        <td>Australia</td>
                        <td class="text-right">
                          750
                        </td>
                      </tr>
                      <tr>
                        <td>
                          <div class="flag">
                            <img src="static/img/flags/GB.png" alt="GB">
                          </div>
                        </td>
                        <td>United Kingdom</td>
                        <td class="text-right">
                          690
                        </td>
                      </tr>
                      <tr>
                        <td>
                          <div class="flag">
                            <img src="static/img/flags/RO.png" alt="RO">
                          </div>
                        </td>
                        <td>Romania</td>
                        <td class="text-right">
                          600
                        </td>
                      </tr>
                      <tr>
                        <td>
                          <div class="flag">
                            <img src="static/img/flags/BR.png" alt="BR">
                          </div>
                        </td>
                        <td>Brasil</td>
                        <td class="text-right">
                          3550
                        </td>
                      </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
    </div>
  </div>
</template>
<script>
  import CircleChartCard from 'src/components/UIComponents/Cards/CircleChartCard.vue'
  import ChartCard from 'src/components/UIComponents/Cards/ChartCard';
  import StatsCard from 'src/components/UIComponents/Cards/StatsCard';
  import {Badge} from 'src/components/UIComponents'
  import Loading from 'src/components/Dashboard/Layout/LoadingMainPanel.vue'
  import TaskList from './Widgets/TaskList'
  const WorldMap = () => ({
    component: import('./../Maps/WorldMap.vue'),
    loading: Loading,
    delay: 200
  })

  export default {
    components: {
      StatsCard,
      WorldMap
    },
    /**
     * Chart data used to render stats, charts. Should be replaced with server data
     */
    data () {
      return {
        statsCards: [
          {
            type: 'warning',
            icon: 'nc-icon nc-delivery-fast',
            title: 'Veículos Disponíveis',
            value: '354',
            footerText: 'Atualizado hoje',
            footerIcon: 'nc-icon nc-refresh-69'
          },
          {
            type: 'success',
            icon: 'nc-icon nc-money-coins',
            title: 'Media de diária',
            value: 'R$103,45',
            footerText: 'Ontem',
            footerIcon: 'nc-icon nc-calendar-60'
          },
          {
            type: 'danger',
            icon: 'nc-icon nc-vector',
            title: 'Regiões Disponíveis',
            value: '14',
            footerText: 'Semana Passada',
            footerIcon: 'nc-icon nc-bell-55'
          },
          {
            type: 'info',
            icon: 'nc-icon nc-favourite-28',
            title: 'Clientes Satisfeitos',
            value: '+45',
            footerText: 'Atualizado hoje',
            footerIcon: 'nc-icon nc-refresh-69'
          }
        ],
        activeUsersChart: {
          data: {
            labels: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct"],
            series: [542, 480, 430, 550, 530, 453, 380, 434, 568, 610]
          }
        },
        emailsCampaignChart: {
          data: {
            labels: ["12pm", "3pm", "6pm", "9pm", "12am", "3am", "6am", "9am"],
            series: [40, 500, 650, 700, 1200, 1250, 1300, 1900]
          }
        },
        activeCountriesChart: {
          data: {
            labels: ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October"],
            series: [80, 78, 86, 96, 83, 85, 76, 75, 88, 90]
          }
        },
        activityChart: {
          data: {
            labels: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20],
            datasets: [
              {
                label: "Data",
                borderColor: '#fcc468',
                fill: true,
                backgroundColor: '#fcc468',
                hoverBorderColor: '#fcc468',
                borderWidth: 8,
                data: [100, 120, 80, 100, 90, 130, 110, 100, 80, 110, 130, 140, 130, 120, 130, 80, 100, 90, 120, 130],
              },
              {
                label: "Data",
                borderColor: '#4cbdd7',
                fill: true,
                backgroundColor: '#4cbdd7',
                hoverBorderColor: '#4cbdd7',
                borderWidth: 8,
                data: [80, 140, 50, 120, 50, 150, 60, 130, 50, 130, 150, 100, 110, 80, 140, 50, 140, 50, 110, 150],
              }
            ]
          }
        }

      }
    }
  }

</script>
<style>

</style>
