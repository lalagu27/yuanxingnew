<template>
  <div class="bms-dashboard">
    <!-- Header -->
    <div class="bms-header">
      <div class="header-left">
        <div class="logo-wrapper">
          <div class="logo-icon"></div>
          <div class="logo-text">
            <div class="main-title">海能BMS业务管理系统</div>
            <div class="sub-title">Business Management System</div>
          </div>
        </div>
      </div>
      <div class="header-nav">
        <div class="nav-item">勘探开发协同CORP</div>
        <div class="nav-item">勘探运营管理EOM</div>
        <div class="nav-item active">开发生产应用DPAS</div>
        <div class="nav-item">工程管理IE³</div>
        <div class="nav-item">钻完井管理DCM</div>
        <div class="nav-item">储量管理OGRS</div>
      </div>
      <div class="header-right">
        <i class="el-icon-bell"></i>
        <i class="el-icon-chat-dot-square"></i>
        <i class="el-icon-setting"></i>
        <i class="el-icon-user"></i>
        <span class="current-time">{{ currentTime }}</span>
      </div>
    </div>

    <!-- Main Content -->
    <div class="bms-main">
      <!-- Left Panel -->
      <div class="left-panel">
        <!-- News/Updates -->
        <div class="panel-card">
          <div class="card-header">
            <i class="el-icon-s-comment"></i>
            <span>勘探 开发生产 工程</span>
            <span class="date">2025-11-25</span>
          </div>
          <div class="news-content">
            <p>今日，有限公司全球勘探现场共个作业点，其中新增12个，终止上报的14个（日雅），2025年国内沪源计划到场井214口，截至目前，国内沪源上完成198口，正在作业中口，终止上完成102口。</p>
          </div>
        </div>

        <!-- Production Monitoring -->
        <div class="panel-card flex-1">
          <div class="card-header">
            <i class="el-icon-data-analysis"></i>
            <span>有限（中国）勘探工作量</span>
          </div>
          <div class="chart-wrapper">
            <div ref="leftChart1" class="chart-container"></div>
          </div>
        </div>

        <!-- Asset Overview -->
        <div class="panel-card">
          <div class="card-header">
            <i class="el-icon-pie-chart"></i>
            <span>有限（中国）资产</span>
          </div>
          <div class="asset-stats">
            <div class="stat-circle">
              <div ref="assetChart1" class="mini-chart"></div>
            </div>
            <div class="stat-circle">
              <div ref="assetChart2" class="mini-chart"></div>
            </div>
          </div>
        </div>

        <!-- Weather/Oil Price -->
        <div class="panel-card">
          <div class="card-header">
            <i class="el-icon-partly-cloudy"></i>
            <span>油 气</span>
            <span class="date-range">2025-01-01 ~ 2025-11-24</span>
          </div>
          <div ref="oilPriceChart" class="chart-container" style="height: 150px;"></div>
        </div>
      </div>

      <!-- Center Map Area -->
      <div class="center-map">
        <div class="map-controls">
          <el-select v-model="selectedRegion" class="region-select" size="small">
            <el-option label="天津" value="tianjin"></el-option>
            <el-option label="北京" value="beijing"></el-option>
            <el-option label="全国" value="china"></el-option>
          </el-select>
          <div class="map-tabs">
            <div class="map-tab active">平台</div>
            <div class="map-tab">管网</div>
            <div class="map-tab">终端</div>
            <div class="map-tab">船舶</div>
          </div>
        </div>
        <div class="map-container">
          <div ref="mapChart" class="china-map"></div>
          <!-- Map Overlays -->
          <div class="map-stats">
            <div class="map-stat-item">
              <div class="stat-label">连接安全作业大队(天)</div>
              <div class="stat-value">329</div>
            </div>
            <div class="map-stat-item">
              <div class="stat-label">海上连接作业大队(天)</div>
              <div class="stat-value">329</div>
            </div>
            <div class="map-stat-item">
              <div class="stat-label">陆上连接作业大队(天)</div>
              <div class="stat-value">329</div>
            </div>
            <div class="map-stat-item">
              <div class="stat-label">陆地钻井入员(人)</div>
              <div class="stat-value">1017</div>
            </div>
            <div class="map-stat-item">
              <div class="stat-label">海上钻井入员(人)</div>
              <div class="stat-value">1254</div>
            </div>
            <div class="map-stat-item">
              <div class="stat-label">累计安全工日(万)</div>
              <div class="stat-value highlight">3531106</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Right Panel -->
      <div class="right-panel">
        <!-- Production Status -->
        <div class="panel-card">
          <div class="card-header">
            <i class="el-icon-s-data"></i>
            <span>有限（中国）产能建设</span>
          </div>
          <div class="production-status">
            <div class="status-row">
              <div class="status-label">
                <span class="dot"></span>
                本开工未完成
              </div>
              <div class="status-label active">
                <span class="dot"></span>
                已开工已完成
              </div>
            </div>
            <div class="donut-charts">
              <div class="donut-item">
                <div ref="donut1" class="donut-chart"></div>
                <div class="donut-label">年度建设产能</div>
              </div>
              <div class="donut-item">
                <div ref="donut2" class="donut-chart"></div>
                <div class="donut-label">年度建成产能</div>
              </div>
              <div class="donut-item">
                <div ref="donut3" class="donut-chart"></div>
                <div class="donut-label">年度总成部署</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Drilling Operations -->
        <div class="panel-card flex-1">
          <div class="card-header">
            <i class="el-icon-location"></i>
            <span>有限（中国）钻完井作业宣</span>
          </div>
          <div class="drilling-ops">
            <div class="ops-tabs">
              <div class="ops-tab active">海上钻井</div>
              <div class="ops-tab">陆上钻井</div>
            </div>
            <div class="gauge-wrapper">
              <div ref="gaugeChart" class="gauge-chart"></div>
            </div>
            <div class="ops-stats">
              <div class="ops-stat-row">
                <span class="ops-label">本年累计钻井计划 (口)</span>
                <span class="ops-value">973</span>
              </div>
              <div class="ops-stat-row">
                <span class="ops-label">本年累计钻井数 (口)</span>
                <span class="ops-value">942</span>
              </div>
            </div>
            <div class="detail-stats">
              <div class="detail-item">
                <div class="detail-label">海上当日在钻数（口）</div>
                <div class="detail-value">56</div>
              </div>
              <div class="detail-item">
                <div class="detail-label">海上当日开钻数（口）</div>
                <div class="detail-value">6072.2</div>
              </div>
              <div class="detail-item">
                <div class="detail-label">海上当日完井钻数（米）</div>
                <div class="detail-value">2534868.45</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Equipment Usage -->
        <div class="panel-card">
          <div class="card-header">
            <i class="el-icon-office-building"></i>
            <span>有限（中国）钻完井设备使用情况</span>
          </div>
          <div class="equipment-usage">
            <div class="usage-item">
              <i class="el-icon-ship"></i>
              <div class="usage-info">
                <div class="usage-label">固定式钻井船</div>
                <div class="usage-value">36</div>
              </div>
            </div>
            <div class="usage-item">
              <i class="el-icon-truck"></i>
              <div class="usage-info">
                <div class="usage-label">半潜式钻井船</div>
                <div class="usage-value">5</div>
              </div>
            </div>
            <div class="usage-item">
              <i class="el-icon-platform"></i>
              <div class="usage-info">
                <div class="usage-label">钻修船</div>
                <div class="usage-value">21</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Storage -->
        <div class="panel-card">
          <div class="card-header">
            <i class="el-icon-files"></i>
            <span>有限（中国）储量</span>
          </div>
          <div class="storage-tabs">
            <div class="storage-tab active">国家</div>
            <div class="storage-tab">内部</div>
          </div>
          <div class="storage-values">
            <div class="storage-item">
              <i class="el-icon-coin"></i>
              <div class="storage-info">
                <div class="storage-label">石油探明地质储量</div>
                <div class="storage-value">707981<span class="unit">万吨</span></div>
              </div>
            </div>
            <div class="storage-item">
              <i class="el-icon-share"></i>
              <div class="storage-info">
                <div class="storage-label">天然气探明地质储量</div>
                <div class="storage-value">24990<span class="unit">立方</span></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  name: 'EnhancedDashboard',
  data() {
    return {
      currentTime: '',
      selectedRegion: 'tianjin'
    }
  },
  mounted() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
    this.$nextTick(() => {
      this.initCharts();
    });
    window.addEventListener('resize', this.handleResize);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    updateTime() {
      const now = new Date();
      this.currentTime = now.getFullYear() + '.' +
        String(now.getMonth()+1).padStart(2,'0') + '.' +
        String(now.getDate()).padStart(2,'0');
    },
    handleResize() {
      const refs = ['leftChart1', 'assetChart1', 'assetChart2', 'oilPriceChart', 'mapChart', 'donut1', 'donut2', 'donut3', 'gaugeChart'];
      refs.forEach(ref => {
        if (this.$refs[ref]) {
          const instance = echarts.getInstanceByDom(this.$refs[ref]);
          if (instance) instance.resize();
        }
      });
    },
    initCharts() {
      // Left Chart 1 - Production Monitoring
      const lc1 = echarts.init(this.$refs.leftChart1);
      lc1.setOption({
        tooltip: { trigger: 'axis' },
        grid: { top: 30, bottom: 20, left: 40, right: 20 },
        legend: {
          data: ['探井个数', '年度目标'],
          textStyle: { color: '#a6d5fa', fontSize: 11 },
          top: 0
        },
        xAxis: {
          type: 'category',
          data: ['探井', '开发井', '调整井'],
          axisLine: { lineStyle: { color: '#1e5a8e' } },
          axisLabel: { color: '#6ea8d3', fontSize: 11 }
        },
        yAxis: {
          type: 'value',
          splitLine: { lineStyle: { color: '#1e5a8e', type: 'dashed' } },
          axisLabel: { color: '#6ea8d3', fontSize: 11 }
        },
        series: [
          {
            name: '探井个数',
            type: 'bar',
            data: [10830.24, 8640, 7320],
            barWidth: 15,
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: '#00d4ff' },
                { offset: 1, color: '#0066cc' }
              ]),
              borderRadius: [4, 4, 0, 0]
            }
          },
          {
            name: '年度目标',
            type: 'bar',
            data: [304, 280, 260],
            barWidth: 15,
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: '#ff9800' },
                { offset: 1, color: '#cc5500' }
              ]),
              borderRadius: [4, 4, 0, 0]
            }
          }
        ]
      });

      // Asset Charts - Donuts
      const createDonut = (ref, value, color1, color2) => {
        const chart = echarts.init(this.$refs[ref]);
        chart.setOption({
          series: [{
            type: 'pie',
            radius: ['60%', '80%'],
            silent: true,
            label: {
              show: true,
              position: 'center',
              formatter: `{value|${value}%}\n{label|万吨}`,
              rich: {
                value: { fontSize: 16, color: '#fff', fontWeight: 'bold', lineHeight: 20 },
                label: { fontSize: 10, color: '#6ea8d3', lineHeight: 16 }
              }
            },
            data: [
              {
                value: value,
                itemStyle: {
                  color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                    { offset: 0, color: color1 },
                    { offset: 1, color: color2 }
                  ])
                }
              },
              {
                value: 100 - value,
                itemStyle: { color: '#1a3a5a', opacity: 0.3 }
              }
            ]
          }]
        });
      };

      createDonut('assetChart1', 56.44, '#00d4ff', '#0088ff');
      createDonut('assetChart2', 25.96, '#ff9800', '#ff5500');

      // Oil Price Chart
      const oilChart = echarts.init(this.$refs.oilPriceChart);
      oilChart.setOption({
        grid: { top: 20, bottom: 30, left: 50, right: 30 },
        tooltip: { trigger: 'axis' },
        legend: {
          data: ['计划日产量', '实际日生成量'],
          textStyle: { color: '#6ea8d3', fontSize: 10 },
          top: 0,
          right: 0
        },
        xAxis: {
          type: 'category',
          data: ['01-01', '03-03', '05-03', '07-03', '09-02', '11-24'],
          axisLine: { lineStyle: { color: '#1e5a8e' } },
          axisLabel: { color: '#6ea8d3', fontSize: 10 }
        },
        yAxis: [
          {
            type: 'value',
            name: '产量',
            position: 'left',
            axisLabel: { color: '#6ea8d3', fontSize: 10 },
            splitLine: { lineStyle: { color: '#1e5a8e', type: 'dashed' } }
          },
          {
            type: 'value',
            name: '万元',
            position: 'right',
            axisLabel: { color: '#6ea8d3', fontSize: 10 },
            splitLine: { show: false }
          }
        ],
        series: [
          {
            name: '计划日产量',
            type: 'line',
            smooth: true,
            data: [120000, 135000, 150000, 148000, 145000, 160000],
            lineStyle: { width: 2, color: '#00d4ff' },
            itemStyle: { color: '#00d4ff' },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: 'rgba(0, 212, 255, 0.3)' },
                { offset: 1, color: 'rgba(0, 212, 255, 0)' }
              ])
            }
          },
          {
            name: '实际日生成量',
            type: 'line',
            smooth: true,
            yAxisIndex: 1,
            data: [3000, 4000, 5000, 4800, 4500, 6000],
            lineStyle: { width: 2, color: '#76ff03' },
            itemStyle: { color: '#76ff03' }
          }
        ]
      });

      // Map Chart (simplified)
      const mapChart = echarts.init(this.$refs.mapChart);
      mapChart.setOption({
        geo: {
          map: 'china',
          roam: false,
          itemStyle: {
            areaColor: '#0a2540',
            borderColor: '#1e5a8e'
          },
          emphasis: {
            itemStyle: {
              areaColor: '#1a4070'
            }
          }
        },
        series: [
          {
            type: 'scatter',
            coordinateSystem: 'geo',
            data: [
              { name: '北京', value: [116.4, 39.9, 100] },
              { name: '天津', value: [117.2, 39.1, 150] },
              { name: '上海', value: [121.5, 31.2, 120] },
              { name: '广州', value: [113.2, 23.1, 80] }
            ],
            symbolSize: val => val[2] / 5,
            itemStyle: {
              color: '#00d4ff',
              shadowBlur: 10,
              shadowColor: '#00d4ff'
            },
            label: {
              show: true,
              formatter: '{b}',
              position: 'top',
              color: '#fff',
              fontSize: 11
            }
          }
        ]
      });

      // Donuts - Production Status
      const createProdDonut = (ref, complete, total, color) => {
        const chart = echarts.init(this.$refs[ref]);
        const percent = ((complete / total) * 100).toFixed(2);
        chart.setOption({
          series: [{
            type: 'pie',
            radius: ['55%', '75%'],
            label: {
              show: true,
              position: 'center',
              formatter: `{percent|${percent}%}\n{value|${complete}}\n{total|${total}}`,
              rich: {
                percent: { fontSize: 14, color: '#fff', fontWeight: 'bold', lineHeight: 22 },
                value: { fontSize: 12, color: color, lineHeight: 18 },
                total: { fontSize: 10, color: '#6ea8d3', lineHeight: 16 }
              }
            },
            data: [
              {
                value: complete,
                name: '已完成',
                itemStyle: {
                  color: new echarts.graphic.LinearGradient(0, 0, 1, 1, [
                    { offset: 0, color: color },
                    { offset: 1, color: color + '88' }
                  ])
                }
              },
              {
                value: total - complete,
                name: '未完成',
                itemStyle: { color: '#1a3a5a' }
              }
            ]
          }]
        });
      };

      createProdDonut('donut1', 2309.52, 4078.86, '#00d4ff');
      createProdDonut('donut2', 1080.06, 1515.68, '#ff9800');
      createProdDonut('donut3', 260.38, 407.89, '#76ff03');

      // Gauge Chart
      const gaugeChart = echarts.init(this.$refs.gaugeChart);
      gaugeChart.setOption({
        series: [{
          type: 'gauge',
          radius: '80%',
          startAngle: 225,
          endAngle: -45,
          min: 0,
          max: 1000,
          axisLine: {
            lineStyle: {
              width: 20,
              color: [
                [0.8, new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                  { offset: 0, color: '#00d4ff' },
                  { offset: 1, color: '#0066cc' }
                ])],
                [0.95, '#ff9800'],
                [1, '#ff5252']
              ]
            }
          },
          pointer: {
            itemStyle: { color: '#00d4ff' },
            width: 4,
            length: '70%'
          },
          axisTick: { distance: -20, length: 6, lineStyle: { color: '#fff', width: 1 } },
          splitLine: { distance: -25, length: 12, lineStyle: { color: '#fff', width: 2 } },
          axisLabel: {
            color: '#6ea8d3',
            distance: 20,
            fontSize: 10
          },
          detail: {
            valueAnimation: true,
            formatter: '{value}',
            color: '#fff',
            fontSize: 24,
            offsetCenter: [0, '70%']
          },
          title: {
            show: true,
            offsetCenter: [0, '90%'],
            color: '#6ea8d3',
            fontSize: 12
          },
          data: [{ value: 942, name: '完成率 96.81%' }]
        }]
      });
    }
  }
}
</script>

<style scoped>
/* Color Scheme - Based on BMS System */
:root {
  --bg-primary: #0a1929;
  --bg-secondary: #0d1b2a;
  --bg-card: rgba(10, 40, 70, 0.6);
  --border-primary: #1e5a8e;
  --border-highlight: #00d4ff;
  --text-primary: #ffffff;
  --text-secondary: #6ea8d3;
  --text-muted: #4a6b8a;
  --accent-blue: #00d4ff;
  --accent-orange: #ff9800;
  --accent-green: #76ff03;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.bms-dashboard {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(180deg, #0a1929 0%, #0d1b2a 50%, #0a1520 100%);
  color: var(--text-primary);
  font-family: 'Microsoft YaHei', 'PingFang SC', sans-serif;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Header */
.bms-header {
  height: 60px;
  background: rgba(10, 25, 41, 0.9);
  border-bottom: 2px solid var(--border-primary);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
}

.header-left {
  display: flex;
  align-items: center;
}

.logo-wrapper {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo-icon {
  width: 40px;
  height: 40px;
  background: linear-gradient(135deg, #00d4ff, #0066cc);
  border-radius: 50%;
  box-shadow: 0 0 15px rgba(0, 212, 255, 0.5);
}

.logo-text .main-title {
  font-size: 20px;
  font-weight: bold;
  color: var(--text-primary);
  letter-spacing: 1px;
}

.logo-text .sub-title {
  font-size: 10px;
  color: var(--text-secondary);
  letter-spacing: 0.5px;
}

.header-nav {
  display: flex;
  gap: 5px;
}

.nav-item {
  padding: 8px 16px;
  font-size: 13px;
  color: var(--text-secondary);
  background: rgba(30, 90, 142, 0.2);
  border: 1px solid rgba(30, 90, 142, 0.5);
  cursor: pointer;
  transition: all 0.3s;
}

.nav-item:hover,
.nav-item.active {
  color: var(--text-primary);
  background: rgba(0, 212, 255, 0.2);
  border-color: var(--border-highlight);
  box-shadow: 0 0 10px rgba(0, 212, 255, 0.3);
}

.header-right {
  display: flex;
  align-items: center;
  gap: 15px;
  color: var(--text-secondary);
  font-size: 16px;
}

.header-right i {
  cursor: pointer;
  transition: color 0.3s;
}

.header-right i:hover {
  color: var(--border-highlight);
}

.current-time {
  font-size: 14px;
  font-family: 'Consolas', monospace;
  color: var(--border-highlight);
  margin-left: 10px;
}

/* Main Content */
.bms-main {
  flex: 1;
  display: flex;
  gap: 10px;
  padding: 10px;
  overflow: hidden;
}

/* Panel Styles */
.left-panel,
.right-panel {
  width: 320px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  overflow-y: auto;
}

.center-map {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
}

/* Card Styles */
.panel-card {
  background: var(--bg-card);
  border: 1px solid var(--border-primary);
  border-radius: 4px;
  padding: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  position: relative;
  backdrop-filter: blur(5px);
}

.panel-card::before {
  content: '';
  position: absolute;
  top: -1px;
  left: -1px;
  width: 12px;
  height: 12px;
  border-top: 2px solid var(--border-highlight);
  border-left: 2px solid var(--border-highlight);
}

.panel-card::after {
  content: '';
  position: absolute;
  bottom: -1px;
  right: -1px;
  width: 12px;
  height: 12px;
  border-bottom: 2px solid var(--border-highlight);
  border-right: 2px solid var(--border-highlight);
}

.flex-1 {
  flex: 1;
  min-height: 0;
}

.card-header {
  display: flex;
  align-items: center;
  gap: 8px;
  padding-bottom: 8px;
  margin-bottom: 10px;
  border-bottom: 1px solid var(--border-primary);
  font-size: 13px;
  color: var(--border-highlight);
  font-weight: bold;
}

.card-header i {
  font-size: 16px;
}

.date,
.date-range {
  margin-left: auto;
  font-size: 11px;
  color: var(--text-muted);
  font-weight: normal;
}

/* News Content */
.news-content {
  font-size: 12px;
  line-height: 1.6;
  color: var(--text-secondary);
  text-align: justify;
}

/* Charts */
.chart-wrapper,
.chart-container {
  width: 100%;
  flex: 1;
  min-height: 150px;
}

.mini-chart {
  width: 100%;
  height: 100px;
}

/* Asset Stats */
.asset-stats {
  display: flex;
  gap: 15px;
  justify-content: space-around;
}

.stat-circle {
  flex: 1;
}

/* Map Controls */
.map-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: var(--bg-card);
  border: 1px solid var(--border-primary);
  border-radius: 4px;
  margin-bottom: 10px;
}

.region-select {
  width: 120px;
}

.map-tabs {
  display: flex;
  gap: 5px;
}

.map-tab {
  padding: 6px 16px;
  font-size: 12px;
  background: rgba(30, 90, 142, 0.3);
  border: 1px solid var(--border-primary);
  color: var(--text-secondary);
  cursor: pointer;
  transition: all 0.3s;
}

.map-tab.active,
.map-tab:hover {
  background: rgba(0, 212, 255, 0.2);
  border-color: var(--border-highlight);
  color: var(--text-primary);
}

/* Map Container */
.map-container {
  flex: 1;
  position: relative;
  background: var(--bg-card);
  border: 1px solid var(--border-primary);
  border-radius: 4px;
  overflow: hidden;
}

.china-map {
  width: 100%;
  height: 100%;
}

.map-stats {
  position: absolute;
  left: 20px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.map-stat-item {
  background: rgba(10, 40, 70, 0.8);
  padding: 8px 12px;
  border-left: 3px solid var(--accent-blue);
  backdrop-filter: blur(5px);
}

.stat-label {
  font-size: 11px;
  color: var(--text-secondary);
  margin-bottom: 4px;
}

.stat-value {
  font-size: 16px;
  font-weight: bold;
  color: var(--accent-blue);
  font-family: 'Consolas', monospace;
}

.stat-value.highlight {
  color: var(--accent-orange);
  font-size: 18px;
}

/* Production Status */
.production-status {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.status-row {
  display: flex;
  gap: 20px;
  font-size: 11px;
}

.status-label {
  display: flex;
  align-items: center;
  gap: 5px;
  color: var(--text-secondary);
}

.status-label.active {
  color: var(--text-primary);
}

.status-label .dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent-orange);
}

.status-label.active .dot {
  background: var(--accent-blue);
}

.donut-charts {
  display: flex;
  gap: 10px;
  justify-content: space-between;
}

.donut-item {
  flex: 1;
  text-align: center;
}

.donut-chart {
  height: 100px;
}

.donut-label {
  font-size: 11px;
  color: var(--text-secondary);
  margin-top: 5px;
}

/* Drilling Operations */
.drilling-ops {
  display: flex;
  flex-direction: column;
  gap: 10px;
  height: 100%;
}

.ops-tabs {
  display: flex;
  gap: 5px;
}

.ops-tab {
  flex: 1;
  padding: 6px;
  text-align: center;
  font-size: 12px;
  background: rgba(30, 90, 142, 0.2);
  border: 1px solid var(--border-primary);
  color: var(--text-secondary);
  cursor: pointer;
  transition: all 0.3s;
}

.ops-tab.active,
.ops-tab:hover {
  background: rgba(0, 212, 255, 0.2);
  border-color: var(--border-highlight);
  color: var(--text-primary);
}

.gauge-wrapper {
  flex: 1;
  min-height: 150px;
}

.gauge-chart {
  width: 100%;
  height: 100%;
}

.ops-stats {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 10px 0;
  border-top: 1px solid var(--border-primary);
  border-bottom: 1px solid var(--border-primary);
}

.ops-stat-row {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
}

.ops-label {
  color: var(--text-secondary);
}

.ops-value {
  color: var(--accent-blue);
  font-weight: bold;
  font-family: 'Consolas', monospace;
}

.detail-stats {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.detail-item {
  display: flex;
  justify-content: space-between;
  font-size: 11px;
}

.detail-label {
  color: var(--text-secondary);
}

.detail-value {
  color: var(--text-primary);
  font-weight: bold;
}

/* Equipment Usage */
.equipment-usage {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.usage-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px;
  background: rgba(30, 90, 142, 0.1);
  border-left: 3px solid var(--accent-blue);
}

.usage-item i {
  font-size: 24px;
  color: var(--accent-blue);
}

.usage-info {
  flex: 1;
}

.usage-label {
  font-size: 11px;
  color: var(--text-secondary);
  margin-bottom: 4px;
}

.usage-value {
  font-size: 18px;
  font-weight: bold;
  color: var(--text-primary);
}

/* Storage */
.storage-tabs {
  display: flex;
  gap: 5px;
  margin-bottom: 10px;
}

.storage-tab {
  flex: 1;
  padding: 5px;
  text-align: center;
  font-size: 11px;
  background: rgba(30, 90, 142, 0.2);
  border: 1px solid var(--border-primary);
  color: var(--text-secondary);
  cursor: pointer;
  transition: all 0.3s;
}

.storage-tab.active,
.storage-tab:hover {
  background: rgba(0, 212, 255, 0.2);
  border-color: var(--border-highlight);
  color: var(--text-primary);
}

.storage-values {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.storage-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px;
  background: rgba(30, 90, 142, 0.1);
  border-left: 3px solid var(--accent-green);
}

.storage-item i {
  font-size: 24px;
  color: var(--accent-green);
}

.storage-info {
  flex: 1;
}

.storage-label {
  font-size: 11px;
  color: var(--text-secondary);
  margin-bottom: 4px;
}

.storage-value {
  font-size: 20px;
  font-weight: bold;
  color: var(--text-primary);
  font-family: 'Consolas', monospace;
}

.unit {
  font-size: 12px;
  color: var(--text-secondary);
  margin-left: 4px;
}

/* Scrollbar */
::-webkit-scrollbar {
  width: 4px;
  height: 4px;
}

::-webkit-scrollbar-track {
  background: rgba(30, 90, 142, 0.1);
}

::-webkit-scrollbar-thumb {
  background: var(--border-primary);
  border-radius: 2px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--border-highlight);
}

/* Element UI Overrides */
.bms-dashboard >>> .el-select .el-input__inner {
  background: rgba(10, 40, 70, 0.6);
  border-color: var(--border-primary);
  color: var(--text-primary);
}

.bms-dashboard >>> .el-select .el-input__inner:hover,
.bms-dashboard >>> .el-select .el-input__inner:focus {
  border-color: var(--border-highlight);
}

.bms-dashboard >>> .el-select-dropdown {
  background: var(--bg-secondary);
  border-color: var(--border-primary);
}

.bms-dashboard >>> .el-select-dropdown__item {
  color: var(--text-secondary);
}

.bms-dashboard >>> .el-select-dropdown__item:hover,
.bms-dashboard >>> .el-select-dropdown__item.selected {
  background: rgba(0, 212, 255, 0.2);
  color: var(--text-primary);
}
</style>
