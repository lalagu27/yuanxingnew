<template>
  <div class="dashboard-container">
    <div class="background-grid"></div>
    <div class="header">
      <div class="header-center">
        <div class="header-title">数据可视化指挥中心</div>
        <div class="header-subtitle">DATA VISUALIZATION COMMAND CENTER</div>
      </div>
      <div class="header-time">{{ currentTime }}</div>
    </div>

    <div class="main-body">
      <!-- Left Sidebar -->
      <div class="left-sidebar">
        <!-- Personal Info -->
        <div class="tech-box user-info-box">
           <div class="tech-box-title">
              <span class="icon">➜</span> 指挥官信息
           </div>
           <div class="user-content">
              <div class="avatar-section">
                  <div class="avatar-ring">
                     <div class="avatar-inner">
                        <i class="el-icon-user"></i>
                     </div>
                  </div>
                  <div class="status-badge">ONLINE</div>
              </div>
              <div class="user-details-list">
                  <div class="detail-row">
                      <span class="label">姓名:</span>
                      <span class="value">张三丰</span>
                  </div>
                  <div class="detail-row">
                      <span class="label">职务:</span>
                      <span class="value">高级指挥长</span>
                  </div>
                  <div class="detail-row">
                      <span class="label">工号:</span>
                      <span class="value tech-font">NO.9527</span>
                  </div>
                  <div class="detail-row">
                      <span class="label">部门:</span>
                      <span class="value">勘探开发指挥中心</span>
                  </div>
              </div>
           </div>
           <div class="action-grid">
               <div class="action-item">公告通知</div>
               <div class="action-item active">重点工作</div>
               <div class="action-item">待办事项</div>
           </div>
        </div>

        <!-- Left Chart Area -->
        <div class="tech-box full-height-box">
           <div class="tech-box-title">
              <span class="icon">➜</span> 生产监控
           </div>
           <div class="chart-column">
              <!-- 1. Oil & Gas Production -->
              <div class="sub-chart-box">
                <div class="sub-title">油气总产量</div>
                <div ref="leftChart1" class="chart-ref"></div>
              </div>
              <!-- 2. Oil & Gas Net Production -->
              <div class="sub-chart-box">
                <div class="sub-title">油气净产量 (万吨)</div>
                <div ref="leftChart2" class="chart-ref"></div>
              </div>
              <!-- 3. Investment -->
              <div class="sub-chart-box">
                <div class="sub-title">投资构成</div>
                <div ref="leftChart3" class="chart-ref"></div>
              </div>
              <!-- 4. Workload -->
              <div class="sub-chart-box">
                <div class="sub-title">工作量统计</div>
                <div ref="leftChart4" class="chart-ref"></div>
              </div>
               <!-- 5. Expenses -->
              <div class="sub-chart-box">
                <div class="sub-title">经费支出</div>
                <div ref="leftChart5" class="chart-ref"></div>
              </div>
           </div>
        </div>
      </div>

      <!-- Right Content -->
      <div class="right-content">
        <!-- Top Cards -->
        <div class="top-cards-row">
          <div class="stat-card" v-for="(card, index) in topCards" :key="index">
            <div class="stat-card-inner">
                <div class="card-icon-box">
                    <i class="el-icon-bell" v-if="index===0"></i>
                    <i class="el-icon-s-check" v-if="index===1"></i>
                    <i class="el-icon-position" v-if="index===2"></i>
                    <i class="el-icon-reading" v-if="index===3"></i>
                    <i class="el-icon-setting" v-if="index===4"></i>
                </div>
                <div class="card-info">
                    <div class="stat-title">{{ card.title }}</div>
                    <div class="stat-list">
                        <div v-for="(item, i) in card.items" :key="i" class="stat-item">{{item}}</div>
                    </div>
                </div>
            </div>
            <div class="corner-decor lt"></div>
            <div class="corner-decor rb"></div>
          </div>
        </div>

        <!-- Main Chart Area + Right Extras -->
        <div class="charts-and-extras">
          <!-- Charts Area -->
          <div class="center-area">
             <div class="map-border-box">
                <div class="border-line top-line"></div>
                <div class="border-line bottom-line"></div>

                <div class="filter-row">
                    <div class="section-label">核心指标考核 DASHBOARD</div>
                    <div class="time-tabs">
                        <span class="tab active">YEAR</span>
                        <span class="tab">QTR</span>
                        <span class="tab">MON</span>
                    </div>
                </div>

                <div class="center-grid">
                  <div class="grid-cell">
                    <div class="cell-header-row">
                        <div class="cell-title">增储上产</div>
                        <div class="cell-tabs">
                            <span class="c-tab active">年</span>
                            <span class="c-tab">月</span>
                            <span class="c-tab">日</span>
                        </div>
                    </div>
                    <div ref="chart1" class="cell-chart"></div>
                  </div>
                  <div class="grid-cell">
                    <div class="cell-header-row">
                        <div class="cell-title">成本效益</div>
                        <div class="cell-tabs">
                            <span class="c-tab active">年</span>
                            <span class="c-tab">月</span>
                            <span class="c-tab">日</span>
                        </div>
                    </div>
                    <div ref="chart2" class="cell-chart"></div>
                  </div>
                  <div class="grid-cell">
                    <div class="cell-header-row">
                        <div class="cell-title">战新产业</div>
                        <div class="cell-tabs">
                            <span class="c-tab active">年</span>
                            <span class="c-tab">月</span>
                            <span class="c-tab">日</span>
                        </div>
                    </div>
                    <div ref="chart3" class="cell-chart"></div>
                  </div>
                  <div class="grid-cell">
                    <div class="cell-header-row">
                        <div class="cell-title">关键绩效</div>
                        <div class="cell-tabs">
                            <span class="c-tab active">年</span>
                            <span class="c-tab">月</span>
                            <span class="c-tab">日</span>
                        </div>
                    </div>
                    <div ref="chart4" class="cell-chart"></div>
                  </div>
                </div>
             </div>
          </div>

          <!-- Right Extras -->
          <div class="right-sidebar">
            <div class="tech-box extra-box">
              <div class="tech-box-title"><span class="icon">➜</span> 综合效能</div>
              <div ref="gaugeChart" class="chart-ref"></div>
            </div>
            <div class="tech-box extra-box">
              <div class="tech-box-title"><span class="icon">➜</span> 实时动态</div>
              <div class="news-scroll">
                 <div class="news-row" v-for="i in 5" :key="i">
                    <span class="news-time">10:{{30-i}}</span>
                    <span class="news-text">系统模块 {{i}} 自检完成...</span>
                 </div>
              </div>
            </div>
            <div class="tech-box extra-box">
              <div class="tech-box-title"><span class="icon">➜</span> 系统监控</div>
              <div class="monitor-list">
                 <div class="monitor-item">
                    <span class="label">CPU LOAD</span>
                    <div class="bar-bg"><div class="bar-fill" style="width: 45%; background: #00e5ff;"></div></div>
                    <span class="val">45%</span>
                 </div>
                 <div class="monitor-item">
                    <span class="label">MEMORY</span>
                    <div class="bar-bg"><div class="bar-fill" style="width: 72%; background: #ffea00;"></div></div>
                    <span class="val">72%</span>
                 </div>
                 <div class="monitor-item">
                    <span class="label">NETWORK</span>
                    <div class="bar-bg"><div class="bar-fill" style="width: 28%; background: #76ff03;"></div></div>
                    <span class="val">28%</span>
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
import * as echarts from 'echarts';

// Utilities for gradients
const getLinearGradient = (c1, c2) => new echarts.graphic.LinearGradient(0, 0, 0, 1, [{offset: 0, color: c1}, {offset: 1, color: c2}]);

export default {
  name: 'Dashboard',
  data() {
    return {
      currentTime: '',
      topCards: [
        { title: '今日提醒', badge: 'W', items: ['9:30 勘探会议', '14:00 表彰大会'] },
        { title: '待审批', badge: 'W', items: ['立项审批', '采购审批'] },
        { title: '差旅计划', badge: 'W', items: ['16:00 航班飞北京'] },
        { title: '党务活动', badge: 'M', items: ['本月党小组会议'] },
        { title: '系统通知', badge: 'S', items: ['服务器维护公告'] },
      ]
    }
  },
  mounted() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
    this.initCharts();
    window.addEventListener('resize', this.handleResize);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    updateTime() {
        const now = new Date();
        this.currentTime = now.getFullYear() + '-' +
            String(now.getMonth()+1).padStart(2,'0') + '-' +
            String(now.getDate()).padStart(2,'0') + ' ' +
            String(now.getHours()).padStart(2,'0') + ':' +
            String(now.getMinutes()).padStart(2,'0') + ':' +
            String(now.getSeconds()).padStart(2,'0');
    },
    handleResize() {
        const refs = ['leftChart1', 'leftChart2', 'leftChart3', 'leftChart4', 'leftChart5', 'chart1', 'chart2', 'chart3', 'chart4', 'gaugeChart'];
        refs.forEach(ref => {
            if (this.$refs[ref]) echarts.getInstanceByDom(this.$refs[ref]).resize();
        });
    },
    initCharts() {
      // Common config - BMS Style
      const axisLineColor = 'rgba(30, 90, 142, 0.5)';
      const splitLineColor = 'rgba(30, 90, 142, 0.2)';
      const labelColor = '#6ea8d3';

      const commonGrid = { top: 25, bottom: 5, left: 0, right: 0, containLabel: true };
      const commonXAxis = {
        axisLine: { lineStyle: { color: axisLineColor } },
        axisTick: { show: false },
        axisLabel: { color: labelColor, fontSize: 9 }
      };
      const commonYAxis = {
        splitLine: { lineStyle: { color: splitLineColor, type: 'dashed' } },
        axisLabel: { color: labelColor, fontSize: 9 }
      };

      // --- Left Charts ---

      // 1. Oil & Gas
      const lc1 = echarts.init(this.$refs.leftChart1);
      lc1.setOption({
         tooltip: { trigger: 'axis' },
         grid: { top: 10, bottom: 0, left: 0, right: 10, containLabel: true },
         xAxis: { type: 'value', show: false },
         yAxis: { type: 'category', data: ['天然气', '石油'], axisLine: {show:false}, axisTick:{show:false}, axisLabel:{color:'#fff'} },
         series: [{
             type: 'bar',
             data: [78, 55],
             barWidth: 8,
             itemStyle: { borderRadius: 4, color: getLinearGradient('#00d4ff', '#0066cc') },
             label: { show: true, position: 'right', color: '#fff' },
             showBackground: true,
             backgroundStyle: { color: 'rgba(30, 90, 142, 0.15)', borderRadius: 4 }
         }]
      });

      // 2. Net Production (Pictorial)
      const lc2 = echarts.init(this.$refs.leftChart2);
      lc2.setOption({
          grid: { top: 0, bottom: 0, left: 0, right: 0, containLabel: true },
          xAxis: { show: false, max: 6000 },
          yAxis: { type: 'category', data: ['净产量'], show: false },
          series: [
              {
                  type: 'pictorialBar',
                  symbol: 'rect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolClip: true,
                  symbolSize: [6, 15],
                  data: [4567],
                  itemStyle: { color: '#00d4ff' },
                  label: { show: true, position: 'right', offset: [10, 0], color: '#fff', fontSize: 14, formatter: '{c}' },
                  z: 10
              },
              {
                  type: 'pictorialBar',
                  symbol: 'rect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [6, 15],
                  data: [6000], // Background
                  itemStyle: { color: 'rgba(0, 212, 255, 0.1)' },
                  animation: false,
                  z: 5
              }
          ]
      });

      // 3. Investment (Donut)
      const lc3 = echarts.init(this.$refs.leftChart3);
      lc3.setOption({
          tooltip: { trigger: 'item' },
          legend: { top: 'center', right: 0, orient: 'vertical', textStyle: { color: '#fff', fontSize: 9 }, itemWidth: 8, itemHeight: 8 },
          series: [{
              type: 'pie',
              radius: ['50%', '70%'],
              center: ['30%', '50%'],
              avoidLabelOverlap: false,
              label: { show: false },
              itemStyle: { borderColor: '#000', borderWidth: 2 },
              data: [
                  { value: 3.45, name: '勘探', itemStyle: {color: '#00d4ff'} },
                  { value: 6.78, name: '开发', itemStyle: {color: '#0088ff'} },
                  { value: 0.89, name: '生产', itemStyle: {color: '#ff9800'} },
                  { value: 1.22, name: '其它', itemStyle: {color: '#ffb74d'} }
              ]
          }]
      });

      // 4. Workload (Line/Area)
      const lc4 = echarts.init(this.$refs.leftChart4);
      lc4.setOption({
          grid: commonGrid,
          tooltip: { trigger: 'axis' },
          xAxis: { ...commonXAxis, type: 'category', data: ['探井', '开发', '调整'] },
          yAxis: { ...commonYAxis, type: 'value' },
          series: [{
              type: 'line',
              smooth: true,
              data: [12, 23, 5],
              symbol: 'circle',
              symbolSize: 6,
              itemStyle: { color: '#00d4ff', borderColor: '#fff' },
              lineStyle: { width: 3, color: '#00d4ff' },
              areaStyle: { color: new echarts.graphic.LinearGradient(0,0,0,1, [{offset:0, color: 'rgba(0, 212, 255, 0.5)'}, {offset:1, color: 'rgba(0, 212, 255, 0)'}]) }
          }]
      });

      // 5. Expenses (Rose)
      const lc5 = echarts.init(this.$refs.leftChart5);
      lc5.setOption({
          tooltip: { trigger: 'item' },
          series: [{
              type: 'pie',
              roseType: 'area',
              radius: [10, 40],
              center: ['50%', '50%'],
              itemStyle: { borderRadius: 3 },
              data: [
                  { value: 40, name: '管理', itemStyle: {color:'#00d4ff'} },
                  { value: 30, name: '销售', itemStyle: {color:'#0088ff'} },
                  { value: 20, name: '科研', itemStyle: {color:'#ff9800'} }
              ],
              label: { color: '#fff', fontSize: 9 }
          }]
      });


      // --- Center Charts ---
      const chartProps = {
          grid: { top: 30, bottom: 5, left: 5, right: 5, containLabel: true },
          xAxis: { ...commonXAxis, axisLabel: { color: '#fff', fontSize: 10 } },
          yAxis: { ...commonYAxis, show: false },
          legend: { top: 0, right: 0, textStyle: { color: '#fff', fontSize: 9 }, itemWidth: 10, itemHeight: 4 }
      };

      // C1: Increase Reserves & Production (3 Series Cylinder)
      const c1 = echarts.init(this.$refs.chart1);
      c1.setOption({
          ...chartProps,
          legend: { show: true, textStyle: {color: '#fff', fontSize: 8}, top: 0, right: 0, itemWidth: 8, itemHeight: 8 },
          xAxis: {
              type: 'category',
              data: ['新增储量', '原油产量', '天然气'],
              axisLabel: {color:'#fff', fontSize: 9, interval: 0}
          },
          grid: { top: 35, bottom: 5, left: 5, right: 5, containLabel: true },
          series: [
              // --- Series 1: Basic (Yellow) ---
              {
                  name: '基本目标',
                  type: 'bar',
                  barWidth: 10,
                  data: [68, 55, 88],
                  itemStyle: { color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{offset: 0, color: '#ffc107'}, {offset: 1, color: '#ff9800'}]) }
              },
              {
                  name: '基本目标',
                  type: 'pictorialBar',
                  symbol: 'circle',
                  symbolPosition: 'end',
                  symbolSize: [10, 5],
                  symbolOffset: ['-110%', -3], // Align with left bar
                  z: 10,
                  data: [68, 55, 88],
                  itemStyle: { color: '#ffc107' },
                  tooltip: { show: false }
              },

              // --- Series 2: Challenge II (Red) ---
              {
                  name: '挑战二档',
                  type: 'bar',
                  barWidth: 10,
                  data: [78, 66, 99],
                  itemStyle: { color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{offset: 0, color: '#ff5252'}, {offset: 1, color: '#d32f2f'}]) }
              },
              {
                  name: '挑战二档',
                  type: 'pictorialBar',
                  symbol: 'circle',
                  symbolPosition: 'end',
                  symbolSize: [10, 5],
                  symbolOffset: [0, -3], // Align with center bar
                  z: 10,
                  data: [78, 66, 99],
                  itemStyle: { color: '#ff5252' },
                  tooltip: { show: false }
              },

              // --- Series 3: Challenge I (Green) ---
              {
                  name: '挑战一档',
                  type: 'bar',
                  barWidth: 10,
                  data: [112, 76, 114.8],
                  itemStyle: { color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{offset: 0, color: '#69f0ae'}, {offset: 1, color: '#00c853'}]) }
              },
              {
                  name: '挑战一档',
                  type: 'pictorialBar',
                  symbol: 'circle',
                  symbolPosition: 'end',
                  symbolSize: [10, 5],
                  symbolOffset: ['110%', -3], // Align with right bar
                  z: 10,
                  data: [112, 76, 114.8],
                  itemStyle: { color: '#69f0ae' },
                  tooltip: { show: false },
                  label: { show: true, position: 'top', color: '#fff', fontSize: 9, offset: [0, -2] }
              }
          ]
      });

      // C2: Cost Efficiency (Digital Block/Meter Style)
      const c2 = echarts.init(this.$refs.chart2);
      c2.setOption({
          ...chartProps,
          legend: { show: true, textStyle: {color: '#fff', fontSize: 8}, top: 0, right: 0, itemWidth: 8, itemHeight: 8 },
          xAxis: {
              type: 'category',
              data: ['利润总额', '桶油五项'],
              axisLabel: {color:'#fff', fontSize: 10},
              axisLine: { show: true, lineStyle: { color: 'rgba(0, 229, 255, 0.3)' } }
          },
          grid: { top: 35, bottom: 5, left: 5, right: 5, containLabel: true },
          series: [
              // --- Series 1: Basic (Pink Blocks) ---
              {
                  name: '基本目标',
                  type: 'pictorialBar',
                  symbol: 'roundRect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [14, 4],
                  symbolClip: true,
                  symbolOffset: ['-140%', 0],
                  data: [45, 20.45],
                  itemStyle: { color: '#ec008c' },
                  label: { show: false }
              },
              // Series 1 Background (faint track)
              {
                  name: '基本目标',
                  type: 'pictorialBar',
                  symbol: 'roundRect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [14, 4],
                  symbolOffset: ['-140%', 0],
                  data: [120, 30], // Max background
                  itemStyle: { color: 'rgba(236, 0, 140, 0.1)' },
                  z: -1,
                  animation: false,
                  tooltip: { show: false },
                  hoverAnimation: false
              },

              // --- Series 2: Challenge II (Orange Blocks) ---
              {
                  name: '挑战二档',
                  type: 'pictorialBar',
                  symbol: 'roundRect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [14, 4],
                  symbolClip: true,
                  symbolOffset: [0, 0],
                  data: [56, 19.14],
                  itemStyle: { color: '#ff9966' },
                  label: { show: false }
              },
              // Background
              {
                  name: '挑战二档',
                  type: 'pictorialBar',
                  symbol: 'roundRect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [14, 4],
                  symbolOffset: [0, 0],
                  data: [120, 30],
                  itemStyle: { color: 'rgba(255, 153, 102, 0.1)' },
                  z: -1,
                  animation: false,
                  tooltip: { show: false },
                  hoverAnimation: false
              },

              // --- Series 3: Challenge I (Blue Blocks) ---
              {
                  name: '挑战一档',
                  type: 'pictorialBar',
                  symbol: 'roundRect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [14, 4],
                  symbolClip: true,
                  symbolOffset: ['140%', 0],
                  data: [112.5, 18.5],
                  itemStyle: { color: '#00c6fb' },
                  label: { show: true, position: 'top', color: '#fff', fontSize: 9, offset: [0, -2] }
              },
              // Background
              {
                  name: '挑战一档',
                  type: 'pictorialBar',
                  symbol: 'roundRect',
                  symbolRepeat: 'fixed',
                  symbolMargin: 2,
                  symbolSize: [14, 4],
                  symbolOffset: ['140%', 0],
                  data: [120, 30],
                  itemStyle: { color: 'rgba(0, 198, 251, 0.1)' },
                  z: -1,
                  animation: false,
                  tooltip: { show: false },
                  hoverAnimation: false
              }
          ]
      });

      // C3: Emerging Industries (3 Series Mountain Peaks)
      const c3 = echarts.init(this.$refs.chart3);
      c3.setOption({
          ...chartProps,
          legend: { show: true, textStyle: {color: '#fff', fontSize: 8}, top: 0, right: 0, itemWidth: 8, itemHeight: 8 },
          xAxis: {
              data: ['投资完成', '营业收入'],
              axisLabel: {color:'#fff', fontSize: 10},
              axisLine: { show: true, lineStyle: { color: 'rgba(0, 229, 255, 0.3)' } },
              axisTick: { show: false }
          },
          grid: { top: 35, bottom: 5, left: 5, right: 5, containLabel: true },
          series: [
              // --- Series 1: Basic (Red) ---
              {
                  name: '基本目标',
                  type: 'pictorialBar',
                  symbol: 'path://M0,10 L5,0 L10,10 z',
                  itemStyle: { color: 'rgba(255, 82, 82, 0.8)' },
                  data: [12, 35],
                  symbolSize: ['50%', '100%'],
                  symbolOffset: ['-60%', 0],
                  z: 10,
                  label: { show: true, position: 'top', fontSize: 10, fontWeight: 'bold', color: '#fff', offset: [0, -5] }
              },

              // --- Series 2: Challenge II (Yellow/Orange) ---
              // Note: Image visually emphasizes 2 main peaks, but legend implies 3. Adding middle layer.
              {
                  name: '挑战二档',
                  type: 'pictorialBar',
                  symbol: 'path://M0,10 L5,0 L10,10 z',
                  itemStyle: { color: 'rgba(255, 179, 0, 0.7)' },
                  data: [18, 55], // Estimated intermediate values
                  symbolSize: ['50%', '100%'],
                  symbolOffset: ['0%', 0],
                  z: 5
              },

              // --- Series 3: Challenge I (Cyan) ---
              {
                  name: '挑战一档',
                  type: 'pictorialBar',
                  symbol: 'path://M0,10 L5,0 L10,10 z',
                  itemStyle: { color: 'rgba(24, 255, 255, 0.8)' },
                  data: [30, 75],
                  symbolSize: ['60%', '100%'],
                  symbolOffset: ['50%', 0],
                  z: 8, // Behind Basic but overlapping
                  label: { show: true, position: 'top', fontSize: 10, fontWeight: 'bold', color: '#18ffff', formatter: '{c}亿元', offset: [0, -5] }
              }
          ]
      });

       // C4: Radar
      const c4 = echarts.init(this.$refs.chart4);
      c4.setOption({
          tooltip: {},
          radar: {
              indicator: [
                  { name: 'A', max: 100 },
                  { name: 'B', max: 100 },
                  { name: 'C', max: 100 },
                  { name: 'D', max: 100 },
                  { name: 'E', max: 100 }
              ],
              splitArea: { areaStyle: { color: ['rgba(0,229,255,0.1)', 'rgba(0,0,0,0)'] } },
              axisLine: { lineStyle: { color: 'rgba(0,229,255,0.3)' } },
              splitLine: { lineStyle: { color: 'rgba(0,229,255,0.3)' } },
              name: { textStyle: { color: '#fff', fontSize: 9 } },
              center: ['50%', '55%'],
              radius: '65%'
          },
          series: [{
              type: 'radar',
              data: [
                  { value: [80, 90, 70, 80, 60], name: 'Data A' },
                  { value: [60, 50, 40, 70, 80], name: 'Data B' }
              ],
              areaStyle: { opacity: 0.3 }
          }]
      });

      // Gauge
      const g1 = echarts.init(this.$refs.gaugeChart);
      g1.setOption({
          series: [{
              type: 'gauge',
              radius: '90%',
              center: ['50%', '60%'],
              startAngle: 180, endAngle: 0,
              min: 0, max: 100,
              splitNumber: 5,
              axisLine: { lineStyle: { width: 10, color: [[1, getLinearGradient('#00d4ff', '#0066cc')]] } },
              pointer: { show: false },
              axisTick: { show: false },
              splitLine: { show: false },
              axisLabel: { show: false },
              detail: {
                  valueAnimation: true,
                  offsetCenter: [0, -10],
                  fontSize: 24,
                  fontWeight: 'bolder',
                  formatter: '{value}%',
                  color: '#fff',
                  textShadowBlur: 10,
                  textShadowColor: '#00d4ff'
              },
              data: [{ value: 98.5 }]
          }]
      });
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap');

/* Animations */
@keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
@keyframes pulse { 0% { opacity: 0.6; } 50% { opacity: 1; } 100% { opacity: 0.6; } }
@keyframes scanline { 0% { top: -100%; } 100% { top: 200%; } }

/* Global Layout */
.dashboard-container {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(180deg, #0a1929 0%, #0d1b2a 50%, #0a1520 100%);
  color: #fff;
  font-family: 'Microsoft YaHei', 'PingFang SC', sans-serif;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  position: relative;
}

/* Header */
.header {
  height: 65px;
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 0 20px;
  background: linear-gradient(180deg, rgba(10, 25, 41, 0.95) 0%, rgba(13, 27, 42, 0.9) 100%);
  border-bottom: 2px solid #1e5a8e;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.5);
  flex-shrink: 0;
  z-index: 10;
}
.header-center {
    width: 100%;
    text-align: center;
    padding-top: 8px;
}
.header-title {
    font-size: 28px;
    font-weight: bold;
    letter-spacing: 3px;
    background: linear-gradient(to bottom, #ffffff, #6ea8d3);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    text-shadow: 0 0 20px rgba(0, 212, 255, 0.3);
}
.header-subtitle {
    font-family: 'Arial', sans-serif;
    font-size: 11px;
    color: #4a8fc7;
    letter-spacing: 2px;
    margin-top: 2px;
    opacity: 0.8;
}
.header-time {
    position: absolute;
    right: 25px;
    top: 20px;
    font-family: 'Consolas', 'Courier New', monospace;
    font-size: 16px;
    color: #00d4ff;
    text-shadow: 0 0 8px rgba(0, 212, 255, 0.6);
    font-weight: bold;
}

/* Main Body */
.main-body {
    flex: 1;
    display: flex;
    padding: 10px;
    gap: 10px;
    overflow: hidden;
}

/* Left Sidebar */
.left-sidebar {
    width: 25%;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

/* Tech Box Style */
.tech-box {
    background: linear-gradient(135deg, rgba(10, 35, 60, 0.7) 0%, rgba(10, 25, 45, 0.6) 100%);
    border: 1px solid rgba(30, 90, 142, 0.4);
    position: relative;
    padding: 12px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(8px);
}
/* Corner Accents */
.tech-box::before {
    content: ''; position: absolute; top: -1px; left: -1px; width: 12px; height: 12px;
    border-top: 2px solid #00d4ff; border-left: 2px solid #00d4ff;
}
.tech-box::after {
    content: ''; position: absolute; bottom: -1px; right: -1px; width: 12px; height: 12px;
    border-bottom: 2px solid #00d4ff; border-right: 2px solid #00d4ff;
}

.tech-box-title {
    font-size: 14px;
    color: #00d4ff;
    border-bottom: 1px solid rgba(30, 90, 142, 0.5);
    padding-bottom: 6px;
    margin-bottom: 10px;
    font-weight: bold;
    display: flex;
    align-items: center;
}
.tech-box-title .icon { margin-right: 6px; font-size: 10px; color: #00d4ff; }

/* User Box */
.user-info-box { height: 180px; display: flex; flex-direction: column; }
.user-content { display: flex; align-items: center; gap: 15px; flex: 1; padding: 0 5px; }

.avatar-section {
    display: flex; flex-direction: column; align-items: center; gap: 5px;
}
.avatar-ring {
    width: 60px; height: 60px; border-radius: 50%;
    border: 2px solid #00d4ff;
    display: flex; align-items: center; justify-content: center;
    animation: spin 10s linear infinite;
    box-shadow: 0 0 15px rgba(0, 212, 255, 0.5);
}
.avatar-inner {
    width: 46px; height: 46px;
    background: linear-gradient(135deg, #00d4ff, #0088ff);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    color: #000; font-size: 24px;
    animation: spin 10s linear infinite reverse;
}
.status-badge {
    display: inline-block; background: rgba(0, 255, 0, 0.2);
    color: #0f0; border: 1px solid #0f0;
    font-size: 9px; padding: 0 5px; border-radius: 2px;
}

.user-details-list {
    flex: 1; display: flex; flex-direction: column; gap: 5px;
}
.detail-row {
    display: flex; align-items: center; font-size: 11px;
    border-bottom: 1px dashed rgba(30, 90, 142, 0.3);
    padding-bottom: 3px;
}
.detail-row .label { color: #6ea8d3; width: 35px; text-align: right; margin-right: 8px; }
.detail-row .value { color: #fff; font-weight: bold; }
.tech-font { font-family: 'Consolas', monospace; color: #00d4ff !important; }

.action-grid { display: flex; gap: 5px; margin-top: 5px; }
.action-item {
    flex: 1; text-align: center;
    background: rgba(30, 90, 142, 0.2);
    font-size: 12px; padding: 6px 0; cursor: pointer;
    border: 1px solid rgba(30, 90, 142, 0.3);
    transition: all 0.3s;
}
.action-item:hover, .action-item.active {
    border-color: #00d4ff; color: #00d4ff;
    background: rgba(0, 212, 255, 0.2);
    box-shadow: 0 0 8px rgba(0, 212, 255, 0.3);
}

/* Left Charts List */
.full-height-box { flex: 1; display: flex; flex-direction: column; }
.chart-column { flex: 1; display: flex; flex-direction: column; gap: 5px; overflow-y: auto; }
.sub-chart-box {
    flex: 1; display: flex; flex-direction: column;
    background: linear-gradient(90deg, rgba(30, 90, 142, 0.1), transparent);
    padding: 6px; border-left: 3px solid rgba(0, 212, 255, 0.4);
}
.sub-title { font-size: 12px; color: #6ea8d3; margin-bottom: 3px; }
.chart-ref { flex: 1; width: 100%; min-height: 50px; }

/* Right Content */
.right-content { flex: 1; display: flex; flex-direction: column; gap: 10px; }

/* Top Stat Cards */
.top-cards-row { display: flex; gap: 10px; height: 80px; }
.stat-card {
    flex: 1; position: relative;
    background: linear-gradient(135deg, rgba(10, 40, 70, 0.7), rgba(10, 30, 55, 0.6));
    border: 1px solid rgba(30, 90, 142, 0.4);
    display: flex; align-items: center;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}
.stat-card-inner { display: flex; align-items: center; padding: 10px; width: 100%; }
.card-icon-box {
    width: 40px; height: 40px;
    background: linear-gradient(135deg, rgba(0, 212, 255, 0.2), rgba(0, 136, 255, 0.1));
    display: flex; align-items: center; justify-content: center;
    font-size: 20px; color: #00d4ff; margin-right: 10px;
    clip-path: polygon(10px 0, 100% 0, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0 100%, 0 10px);
    box-shadow: 0 0 10px rgba(0, 212, 255, 0.3);
}
.stat-title { font-size: 12px; color: #6ea8d3; }
.stat-item { font-size: 11px; color: #fff; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 100px; }

.corner-decor { position: absolute; width: 10px; height: 10px; border-color: #00d4ff; border-style: solid; }
.corner-decor.lt { top: 0; left: 0; border-width: 2px 0 0 2px; }
.corner-decor.rb { bottom: 0; right: 0; border-width: 0 2px 2px 0; }

/* Main Center Area */
.charts-and-extras { flex: 1; display: flex; gap: 10px; }

.center-area { flex: 3; display: flex; flex-direction: column; position: relative; }

.map-border-box {
    flex: 1;
    border: 1px solid rgba(30, 90, 142, 0.4);
    background: linear-gradient(135deg, rgba(10, 30, 50, 0.6), rgba(10, 25, 40, 0.5));
    position: relative; display: flex; flex-direction: column; padding: 20px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
}
/* Complex Center Border Animation */
.border-line { position: absolute; height: 2px; background: #00d4ff; width: 30%; transition: all 0.5s; box-shadow: 0 0 8px #00d4ff; }
.top-line { top: 0; left: 35%; }
.bottom-line { bottom: 0; left: 35%; }

.filter-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
.section-label { font-family: 'Arial'; color: #00d4ff; font-size: 16px; letter-spacing: 2px; font-weight: bold; }
.time-tabs { display: flex; gap: 5px; }
.tab {
    padding: 4px 12px; font-size: 11px;
    background: rgba(30, 90, 142, 0.3);
    border: 1px solid rgba(30, 90, 142, 0.5); cursor: pointer; color: #6ea8d3;
    transition: all 0.3s;
}
.tab.active {
    border-color: #00d4ff; color: #00d4ff;
    background: rgba(0, 212, 255, 0.2);
    box-shadow: 0 0 8px rgba(0, 212, 255, 0.4);
}

.center-grid {
    flex: 1; display: grid;
    grid-template-columns: 1fr 1fr; grid-template-rows: 1fr 1fr;
    gap: 15px;
}
.grid-cell {
    background: linear-gradient(135deg, rgba(10, 40, 70, 0.15), rgba(10, 30, 50, 0.1));
    border: 1px solid rgba(30, 90, 142, 0.3);
    display: flex; flex-direction: column;
    padding: 10px;
}
.cell-header-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 5px;
}
.cell-title {
    font-size: 14px; color: #fff;
    border-left: 3px solid #00d4ff; padding-left: 8px;
    font-weight: bold;
}
.cell-tabs {
    display: flex;
    gap: 3px;
}
.c-tab {
    font-size: 10px;
    padding: 2px 8px;
    background: rgba(30, 90, 142, 0.2);
    border: 1px solid rgba(30, 90, 142, 0.4);
    color: #6ea8d3;
    cursor: pointer;
    border-radius: 2px;
    transition: all 0.3s;
}
.c-tab:hover {
    border-color: #00d4ff;
    color: #00d4ff;
}
.c-tab.active {
    background: rgba(0, 212, 255, 0.25);
    border-color: #00d4ff;
    color: #00d4ff;
    box-shadow: 0 0 6px rgba(0, 212, 255, 0.4);
}
.cell-chart { flex: 1; width: 100%; min-height: 0; }

/* Right Sidebar */
.right-sidebar { width: 25%; display: flex; flex-direction: column; gap: 10px; }
.extra-box { flex: 1; display: flex; flex-direction: column; }

.news-scroll { flex: 1; overflow-y: auto; padding: 5px; }
.news-row {
    font-size: 11px; margin-bottom: 8px;
    border-bottom: 1px dashed rgba(30, 90, 142, 0.3);
    padding-bottom: 4px; display: flex;
}
.news-time { color: #00d4ff; margin-right: 8px; font-family: 'Consolas', monospace; }
.news-text { color: #b8c9d8; }

.monitor-list { display: flex; flex-direction: column; justify-content: space-around; flex: 1; padding: 5px; }
.monitor-item { display: flex; align-items: center; gap: 10px; font-size: 11px; }
.label { width: 60px; color: #6ea8d3; }
.bar-bg { flex: 1; height: 8px; background: rgba(30, 90, 142, 0.3); border-radius: 4px; overflow: hidden; }
.bar-fill { height: 100%; box-shadow: 0 0 8px currentColor; transition: width 0.5s; }
.val { width: 30px; text-align: right; color: #fff; font-family: 'Consolas', monospace; font-weight: bold; }

/* Scrollbar */
::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: rgba(30, 90, 142, 0.1); }
::-webkit-scrollbar-thumb { background: rgba(0, 212, 255, 0.5); border-radius: 2px; }
::-webkit-scrollbar-thumb:hover { background: #00d4ff; }
</style>
