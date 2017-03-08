<template>
  <div class="amBarchart">
  </div>
</template>

<script>
  import 'amcharts3/amcharts/amcharts'
  import 'amcharts3/amcharts/serial'
  import 'amcharts3/amcharts/plugins/export'
  import 'amcharts3/amcharts/plugins/export/export.css'

  let defaultInitOptions = {
    'type': 'serial',
    'theme': 'light',
    'dataProvider': [
      {value: 100, category: 'A'},
      {value: 200, category: 'B'},
      {value: 300, category: 'C'},
      {value: 400, category: 'D'},
      {value: 300, category: 'E'},
      {value: 200, category: 'F'}
    ],
    'startDuration': 0,
    'handDrawn': false,
    'valueAxes': [{
      'gridColor': '#FFFFFF',
      'gridAlpha': 0.2,
      'dashLength': 0
    }],
    'gridAboveGraphs': true,
    'graphs': [{
      'id': 'g1',
      'balloonText': '[[category]]: <b>[[value]]</b>',
      'fillAlphas': 0.8,
      'lineAlpha': 0.2,
      'type': 'column',
      'valueField': 'value'
    }],
    'chartCursor': {
      'categoryBalloonEnabled': false,
      'cursorAlpha': 0,
      'zoomable': false
    },
    'categoryField': 'category',
    'categoryAxis': {
      'gridPosition': 'start',
      'gridAlpha': 0,
      'tickPosition': 'start',
      'tickLength': 20
    },
    'export': {
      'enabled': true
    }
  }

  export default {
    name: 'amBarchart',
    props: {
      options: Object,
      data: Array,
      handDrawn: Boolean
    },
    data () {
      return {
        chart: null,
        initOptions: defaultInitOptions
      }
    },
    methods: {
      _init () {
        console.debug('AmCharts bar init')
        if (this.chart) {
          return
        }

        let chart = window.AmCharts.makeChart(this.$el, this.initOptions)
        this.chart = chart
        this.chart.handDrawn = this.handDrawn
      },
      _setData (newData) {
        this.chart.dataProvider = newData
        this.chart.validateData()
      }
    },
    mounted () {
      this._init()
    },
    watch: {
      data: function (newData) {
        if (this.chart) {
          this._setData(newData)
        } else {
          this._init()
          this._setData(newData)
        }
      }
    }
  }
</script>

<style scoped>
  .amBarchart {
    width: 600px;
    height: 400px;
  }
</style>
