<template>
  <v-col cols="12" md="6" class="DataCard">
    <monitoring-confirmed-cases-chart
      :title="$t('モニタリング指標(1)新規陽性者数')"
      title-id="monitoring-number-of-confirmed-cases"
      chart-id="monitoring-confirmed-cases-chart"
      :chart-data="chartData"
      :date="date"
      :labels="labels"
      :data-labels="dataLabels"
      :table-labels="tableLabels"
      :additional-lines="additionalLines"
      :unit="$t('人')"
      url="https://catalog.data.metro.tokyo.lg.jp/dataset/t000010d0000000068"
    >
      <template v-slot:additionalDescription>
        <ul class="ListStyleNone">
          <li>
            {{ $t('（注）保健所から発生届が提出された日を基準とする') }}
          </li>
          <li>
            {{ $t('（注）医療機関等が行った検査も含む') }}
          </li>
          <li>
            {{
              $t('（注）チャーター機帰国者、クルーズ船乗客等は含まれていない')
            }}
          </li>
          <li>
            {{
              $t(
                '（注）集団感染発生や曜日による件数のばらつきにより、日々の結果が変動するため、こうしたばらつきを平準化し全体の傾向を見る趣旨から、過去７日間の移動平均値を折れ線グラフで示す（たとえば、5月7日の移動平均値は、5月1日から5月7日までの実績値を平均したもの）'
              )
            }}
          </li>
        </ul>
      </template>
    </monitoring-confirmed-cases-chart>
  </v-col>
</template>

<script>
import MonitoringConfirmedCasesChart from '@/components/MonitoringConfirmedCasesChart.vue'
import Data from '@/data/daily_positive_detail.json'

export default {
  components: {
    MonitoringConfirmedCasesChart
  },
  data() {
    const [patientsCount, sevenDayMoveAverages, labels] = Data.data.reduce(
      (res, data) => {
        res[0].push(data.count)
        res[1].push(data.weekly_average_count)
        res[2].push(data.diagnosed_date)
        return res
      },
      [[], [], []]
    )
    const chartData = [patientsCount, sevenDayMoveAverages]
    const dataLabels = [
      this.$t('陽性者数'),
      this.$t('７日間移動平均'),
      this.$t('緩和の目安'),
      this.$t('再要請の目安')
    ]
    const tableLabels = [this.$t('陽性者数'), this.$t('７日間移動平均')]
    const date = Data.date
    const additionalLines = [20, 50]

    return {
      chartData,
      date,
      labels,
      dataLabels,
      tableLabels,
      additionalLines
    }
  }
}
</script>
