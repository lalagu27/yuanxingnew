<template>
  <div class="dashboard-container">
    <div class="background-grid"></div>
    <div class="header">
      <div class="header-center">
        <div class="header-title">指标展示首页</div>
        <div class="header-subtitle">DATA VISUALIZATION COMMAND CENTER</div>
      </div>
      <div class="header-time">{{ currentTime }}</div>
    </div>

    <div class="main-body">
      <!-- Left Sidebar -->
      <div class="left-sidebar">
        <!-- Personal Info -->
        <div class="tech-box user-info-box">
          <div class="tech-box-title"><span class="icon">➜</span> 人员信息</div>
          <div class="user-content">
            <div class="avatar-section">
              <div class="avatar-ring">
                <div class="avatar-inner">
                  <i class="el-icon-user"></i>
                </div>
              </div>
            </div>
            <div class="user-details-grid">
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
        </div>

        <!-- Left Chart Area -->
        <div class="tech-box full-height-box">
          <div class="tech-box-title"><span class="icon">➜</span> 生产监控</div>
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
                <i class="el-icon-bell" v-if="index === 0"></i>
                <i class="el-icon-s-check" v-if="index === 1"></i>
                <i class="el-icon-position" v-if="index === 2"></i>
                <i class="el-icon-reading" v-if="index === 3"></i>
                <i class="el-icon-setting" v-if="index === 4"></i>
              </div>
              <div class="card-info">
                <div class="stat-title">{{ card.title }}</div>
                <div class="stat-list">
                  <div
                    v-for="(item, i) in card.items"
                    :key="i"
                    class="stat-item"
                  >
                    {{ item }}
                  </div>
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
                  <span
                    class="tab"
                    :class="{ active: globalTimeMode === 'year' }"
                    @click="switchGlobalTime('year')"
                    >年</span
                  >
                  <span
                    class="tab"
                    :class="{ active: globalTimeMode === 'month' }"
                    @click="switchGlobalTime('month')"
                    >月</span
                  >
                  <span
                    class="tab"
                    :class="{ active: globalTimeMode === 'day' }"
                    @click="switchGlobalTime('day')"
                    >日</span
                  >
                </div>
              </div>

              <div class="center-grid">
                <div class="grid-cell">
                  <div class="cell-header-row">
                    <div class="cell-title">利润总额</div>
                    <div class="cell-tabs">
                      <span
                        class="c-tab"
                        :class="{ active: profitTimeMode === 'year' }"
                        @click="switchProfitTime('year')"
                        >年</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: profitTimeMode === 'month' }"
                        @click="switchProfitTime('month')"
                        >月</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: profitTimeMode === 'day' }"
                        @click="switchProfitTime('day')"
                        >日</span
                      >
                    </div>
                  </div>
                  <div ref="chart1" class="cell-chart"></div>
                </div>
                <div class="grid-cell">
                  <div class="cell-header-row">
                    <div class="cell-title">桶油五项</div>
                    <div class="cell-tabs">
                      <span
                        class="c-tab"
                        :class="{ active: barrelTimeMode === 'year' }"
                        @click="switchBarrelTime('year')"
                        >年</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: barrelTimeMode === 'month' }"
                        @click="switchBarrelTime('month')"
                        >月</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: barrelTimeMode === 'day' }"
                        @click="switchBarrelTime('day')"
                        >日</span
                      >
                    </div>
                  </div>
                  <div ref="chart2" class="cell-chart"></div>
                </div>
                <div class="grid-cell">
                  <div class="cell-header-row">
                    <div class="cell-title">投资完成</div>
                    <div class="cell-tabs">
                      <span
                        class="c-tab"
                        :class="{ active: investTimeMode === 'year' }"
                        @click="switchInvestTime('year')"
                        >年</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: investTimeMode === 'month' }"
                        @click="switchInvestTime('month')"
                        >月</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: investTimeMode === 'day' }"
                        @click="switchInvestTime('day')"
                        >日</span
                      >
                    </div>
                  </div>
                  <div ref="chart3" class="cell-chart"></div>
                </div>
                <div class="grid-cell">
                  <div class="cell-header-row">
                    <div class="cell-title">营业收入</div>
                    <div class="cell-tabs">
                      <span
                        class="c-tab"
                        :class="{ active: revenueTimeMode === 'year' }"
                        @click="switchRevenueTime('year')"
                        >年</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: revenueTimeMode === 'month' }"
                        @click="switchRevenueTime('month')"
                        >月</span
                      >
                      <span
                        class="c-tab"
                        :class="{ active: revenueTimeMode === 'day' }"
                        @click="switchRevenueTime('day')"
                        >日</span
                      >
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
              <div class="right-chart-header">
                <div class="tech-box-title">
                  <span class="icon">➜</span> 新增经济可采储量
                </div>
                <div class="right-tabs">
                  <span
                    class="r-tab"
                    :class="{ active: reservesTimeMode === 'year' }"
                    @click="switchReservesTime('year')"
                    >年</span
                  >
                  <span
                    class="r-tab"
                    :class="{ active: reservesTimeMode === 'month' }"
                    @click="switchReservesTime('month')"
                    >月</span
                  >
                  <span
                    class="r-tab"
                    :class="{ active: reservesTimeMode === 'day' }"
                    @click="switchReservesTime('day')"
                    >日</span
                  >
                </div>
              </div>
              <div ref="chartReserves" class="chart-ref"></div>
            </div>
            <div class="tech-box extra-box">
              <div class="right-chart-header">
                <div class="tech-box-title">
                  <span class="icon">➜</span> 新增原油总产量
                </div>
                <div class="right-tabs">
                  <span
                    class="r-tab"
                    :class="{ active: oilTimeMode === 'year' }"
                    @click="switchOilTime('year')"
                    >年</span
                  >
                  <span
                    class="r-tab"
                    :class="{ active: oilTimeMode === 'month' }"
                    @click="switchOilTime('month')"
                    >月</span
                  >
                  <span
                    class="r-tab"
                    :class="{ active: oilTimeMode === 'day' }"
                    @click="switchOilTime('day')"
                    >日</span
                  >
                </div>
              </div>
              <div ref="chartOil" class="chart-ref"></div>
            </div>
            <div class="tech-box extra-box">
              <div class="right-chart-header">
                <div class="tech-box-title">
                  <span class="icon">➜</span> 新增天然气总产量
                </div>
                <div class="right-tabs">
                  <span
                    class="r-tab"
                    :class="{ active: gasTimeMode === 'year' }"
                    @click="switchGasTime('year')"
                    >年</span
                  >
                  <span
                    class="r-tab"
                    :class="{ active: gasTimeMode === 'month' }"
                    @click="switchGasTime('month')"
                    >月</span
                  >
                  <span
                    class="r-tab"
                    :class="{ active: gasTimeMode === 'day' }"
                    @click="switchGasTime('day')"
                    >日</span
                  >
                </div>
              </div>
              <div ref="chartGas" class="chart-ref"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as echarts from "echarts";

// Utilities for gradients
const getLinearGradient = (c1, c2) =>
  new echarts.graphic.LinearGradient(0, 0, 0, 1, [
    { offset: 0, color: c1 },
    { offset: 1, color: c2 },
  ]);

export default {
  name: "Dashboard",
  data() {
    return {
      currentTime: "",
      globalTimeMode: "day",
      reservesTimeMode: "day",
      oilTimeMode: "day",
      gasTimeMode: "day",
      profitTimeMode: "day",
      barrelTimeMode: "day",
      revenueTimeMode: "day",
      investTimeMode: "day",
      topCards: [
        {
          title: "今日提醒",
          badge: "W",
          items: ["9:30 勘探会议", "14:00 表彰大会"],
        },
        {
          title: "下周事项",
          badge: "S",
          items: ["周一安全培训", "周三项目评审"],
        },
        { title: "待审批", badge: "W", items: ["立项审批", "采购审批"] },
        { title: "日程管理", badge: "W", items: ["16:00 航班飞北京"] },
        { title: "党务活动", badge: "M", items: ["本月党小组会议"] },
      ],
    };
  },
  mounted() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
    this.initCharts();
    window.addEventListener("resize", this.handleResize);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    updateTime() {
      const now = new Date();
      this.currentTime =
        now.getFullYear() +
        "-" +
        String(now.getMonth() + 1).padStart(2, "0") +
        "-" +
        String(now.getDate()).padStart(2, "0") +
        " " +
        String(now.getHours()).padStart(2, "0") +
        ":" +
        String(now.getMinutes()).padStart(2, "0") +
        ":" +
        String(now.getSeconds()).padStart(2, "0");
    },
    handleResize() {
      const refs = [
        "leftChart1",
        "leftChart2",
        "leftChart3",
        "leftChart4",
        "leftChart5",
        "chart1",
        "chart2",
        "chart3",
        "chart4",
        "chartReserves",
        "chartOil",
        "chartGas",
      ];
      refs.forEach((ref) => {
        if (this.$refs[ref]) {
          const chartInstance = echarts.getInstanceByDom(this.$refs[ref]);
          if (chartInstance) {
            chartInstance.resize();
          }
        }
      });
    },
    // 全局时间切换方法
    switchGlobalTime(mode) {
      this.globalTimeMode = mode;
      // 同步所有图表的时间模式
      this.profitTimeMode = mode;
      this.barrelTimeMode = mode;
      this.investTimeMode = mode;
      this.revenueTimeMode = mode;
      this.reservesTimeMode = mode;
      this.oilTimeMode = mode;
      this.gasTimeMode = mode;
      // 重新初始化所有图表
      this.initProfitChart();
      this.initBarrelChart();
      this.initInvestChart();
      this.initRevenueChart();
      this.initReservesChart();
      this.initOilChart();
      this.initGasChart();
    },
    // 各个图表的独立切换方法
    switchProfitTime(mode) {
      this.profitTimeMode = mode;
      this.initProfitChart();
    },
    switchBarrelTime(mode) {
      this.barrelTimeMode = mode;
      this.initBarrelChart();
    },
    switchInvestTime(mode) {
      this.investTimeMode = mode;
      this.initInvestChart();
    },
    switchRevenueTime(mode) {
      this.revenueTimeMode = mode;
      this.initRevenueChart();
    },
    switchReservesTime(mode) {
      this.reservesTimeMode = mode;
      this.initReservesChart();
    },
    switchOilTime(mode) {
      this.oilTimeMode = mode;
      this.initOilChart();
    },
    switchGasTime(mode) {
      this.gasTimeMode = mode;
      this.initGasChart();
    },
    initCharts() {
      // Common config - BMS Style
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const commonGrid = {
        top: 25,
        bottom: 5,
        left: 0,
        right: 0,
        containLabel: true,
      };
      const commonXAxis = {
        axisLine: { lineStyle: { color: axisLineColor } },
        axisTick: { show: false },
        axisLabel: { color: labelColor, fontSize: 9 },
      };
      const commonYAxis = {
        splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
        axisLabel: { color: labelColor, fontSize: 9 },
      };

      // --- Left Charts ---

      // 1. Oil & Gas
      const lc1 = echarts.init(this.$refs.leftChart1);
      lc1.setOption({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              const unit = item.axisValue === "天然气" ? "亿方" : "万吨";
              result += item.marker + " " + item.value + unit + "<br/>";
            });
            return result;
          },
        },
        grid: { top: 10, bottom: 0, left: 0, right: 50, containLabel: true },
        xAxis: { type: "value", show: false },
        yAxis: {
          type: "category",
          data: ["天然气 (亿方)", "石油 (万吨)"],
          axisLine: { show: false },
          axisTick: { show: false },
          axisLabel: { color: "#fff", fontSize: 11 },
        },
        series: [
          {
            type: "bar",
            data: [78, 55],
            barWidth: 8,
            itemStyle: {
              borderRadius: 4,
              color: getLinearGradient("#00d4ff", "#0066cc"),
            },
            label: {
              show: true,
              position: "right",
              color: "#00d4ff",
              fontSize: 13,
              fontWeight: "bold",
              formatter: function (params) {
                const unit = params.dataIndex === 0 ? "亿方" : "万吨";
                return params.value + unit;
              },
            },
            showBackground: true,
            backgroundStyle: {
              color: "rgba(30, 90, 142, 0.15)",
              borderRadius: 4,
            },
          },
        ],
      });

      // 2. Net Production (Pictorial)
      const lc2 = echarts.init(this.$refs.leftChart2);
      lc2.setOption({
        grid: {
          top: 15,
          bottom: 15,
          left: 10,
          right: 100,
          containLabel: false,
        },
        xAxis: { show: false, max: 6000 },
        yAxis: { type: "category", data: ["净产量"], show: false },
        series: [
          {
            type: "pictorialBar",
            symbol: "rect",
            symbolRepeat: "fixed",
            symbolMargin: 2,
            symbolClip: true,
            symbolSize: [6, 15],
            data: [4567],
            itemStyle: { color: "#00d4ff" },
            label: {
              show: true,
              position: "right",
              offset: [10, 0],
              color: "#00d4ff",
              fontSize: 16,
              fontWeight: "bold",
              formatter: "{c} 万吨",
              fontFamily: "Consolas, monospace",
              textShadowColor: "rgba(0, 212, 255, 0.8)",
              textShadowBlur: 6,
            },
            z: 10,
          },
          {
            type: "pictorialBar",
            symbol: "rect",
            symbolRepeat: "fixed",
            symbolMargin: 2,
            symbolSize: [6, 15],
            data: [6000], // Background
            itemStyle: { color: "rgba(0, 212, 255, 0.1)" },
            animation: false,
            z: 5,
          },
        ],
      });

      // 3. Investment (Donut)
      const lc3 = echarts.init(this.$refs.leftChart3);
      lc3.setOption({
        tooltip: {
          trigger: "item",
          formatter: "{b}: {c}亿元 ({d}%)",
        },
        legend: {
          top: "center",
          right: 0,
          orient: "vertical",
          textStyle: { color: "#fff", fontSize: 9 },
          itemWidth: 8,
          itemHeight: 8,
          formatter: function (name) {
            const data = [
              { name: "勘探", value: 3.45 },
              { name: "开发", value: 6.78 },
              { name: "生产", value: 0.89 },
              { name: "其它", value: 1.22 },
            ];
            const item = data.find((d) => d.name === name);
            return item ? name + " " + item.value + "亿" : name;
          },
        },
        series: [
          {
            type: "pie",
            radius: ["50%", "70%"],
            center: ["30%", "50%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
            },
            labelLine: {
              show: false,
            },
            itemStyle: {
              borderColor: "#0a1929",
              borderWidth: 2,
            },
            emphasis: {
              label: {
                show: true,
                fontSize: 11,
                fontWeight: "bold",
                color: "#fff",
                formatter: "{c}亿",
              },
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 212, 255, 0.5)",
              },
            },
            data: [
              { value: 3.45, name: "勘探", itemStyle: { color: "#00d4ff" } },
              { value: 6.78, name: "开发", itemStyle: { color: "#0088ff" } },
              { value: 0.89, name: "生产", itemStyle: { color: "#ff9800" } },
              { value: 1.22, name: "其它", itemStyle: { color: "#ffb74d" } },
            ],
          },
        ],
      });

      // 4. Workload (Line/Area)
      const lc4 = echarts.init(this.$refs.leftChart4);
      lc4.setOption({
        grid: commonGrid,
        tooltip: {
          trigger: "axis",
          formatter: "{b}: {c}日",
        },
        xAxis: {
          ...commonXAxis,
          type: "category",
          data: ["探井", "开发", "调整"],
        },
        yAxis: {
          ...commonYAxis,
          type: "value",
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}日",
          },
        },
        series: [
          {
            type: "line",
            smooth: true,
            data: [12, 23, 5],
            symbol: "circle",
            symbolSize: 6,
            itemStyle: { color: "#00d4ff", borderColor: "#fff" },
            lineStyle: { width: 3, color: "#00d4ff" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(0, 212, 255, 0.5)" },
                { offset: 1, color: "rgba(0, 212, 255, 0)" },
              ]),
            },
            label: {
              show: true,
              position: "top",
              color: "#00d4ff",
              fontSize: 10,
              formatter: "{c}日",
            },
          },
        ],
      });

      // 5. Expenses (Rose)
      const lc5 = echarts.init(this.$refs.leftChart5);
      lc5.setOption({
        tooltip: {
          trigger: "item",
          formatter: "{b}: {c}亿元 ({d}%)",
        },
        legend: {
          bottom: 0,
          left: "center",
          orient: "horizontal",
          textStyle: { color: "#6ea8d3", fontSize: 7 },
          itemWidth: 6,
          itemHeight: 6,
          itemGap: 5,
          formatter: function (name) {
            const shortNames = {
              行政管理费: "行政",
              分公司行政: "分公司",
              研究院行政: "研究院",
              深海工程中心: "深海",
              销售费用: "销售",
              综合科研: "科研",
            };
            return shortNames[name] || name;
          },
        },
        series: [
          {
            type: "pie",
            roseType: "area",
            radius: [12, 50],
            center: ["50%", "42%"],
            itemStyle: {
              borderRadius: 3,
              borderColor: "#0a1929",
              borderWidth: 2,
            },
            data: [
              {
                value: 4.32,
                name: "行政管理费",
                itemStyle: { color: "#00d4ff" },
              },
              {
                value: 2.1,
                name: "分公司行政",
                itemStyle: { color: "#0088ff" },
              },
              {
                value: 1.8,
                name: "研究院行政",
                itemStyle: { color: "#00aaff" },
              },
              {
                value: 0.42,
                name: "深海工程中心",
                itemStyle: { color: "#40c4ff" },
              },
              {
                value: 2.22,
                name: "销售费用",
                itemStyle: { color: "#ff9800" },
              },
              {
                value: 1.11,
                name: "综合科研",
                itemStyle: { color: "#ffb74d" },
              },
            ],
            label: {
              show: false,
            },
            labelLine: {
              show: false,
            },
            emphasis: {
              label: {
                show: true,
                fontSize: 10,
                fontWeight: "bold",
                color: "#fff",
                formatter: "{b}\n{c}亿",
              },
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 212, 255, 0.5)",
              },
            },
          },
        ],
      });

      // --- Center Charts ---
      const chartProps = {
        grid: { top: 30, bottom: 5, left: 5, right: 5, containLabel: true },
        xAxis: { ...commonXAxis, axisLabel: { color: "#fff", fontSize: 10 } },
        yAxis: { ...commonYAxis, show: false },
        legend: {
          top: 0,
          right: 0,
          textStyle: { color: "#fff", fontSize: 9 },
          itemWidth: 10,
          itemHeight: 4,
        },
      };

      // C1: 利润综合 (Pictorial Bar with Time Switching)
      this.initProfitChart();

      // C2: 桶油五项 (Time Series with Bar Chart)
      this.initBarrelChart();

      // C3: 投资完成 (Time Series with Bar Chart)
      this.initInvestChart();

      // C4: 营业收入 (Time Series with Bar Chart)
      this.initRevenueChart();

      // --- Right Sidebar Charts (Time Series) ---

      // Generate time axis data
      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();

        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getFullYear() + "/" + (d.getMonth() + 1));
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "年");
          }
        }

        return result;
      };

      // Common chart configuration for 3-line score charts
      const createScoreChartOption = (
        timeMode,
        baseData,
        challenge2Data,
        challenge1Data
      ) => ({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          top: 0,
          right: 0,
          textStyle: { color: "#fff", fontSize: 8 },
          itemWidth: 8,
          itemHeight: 8,
        },
        grid: { top: 30, bottom: 20, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: generateTimeAxis(timeMode),
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
          axisLabel: { color: labelColor, fontSize: 8, rotate: 30 },
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          {
            name: "基本目标",
            type: "line",
            smooth: true,
            data: baseData,
            symbol: "circle",
            symbolSize: 4,
            itemStyle: {
              color: "#ffc107",
              borderColor: "#fff",
              borderWidth: 1,
            },
            lineStyle: { width: 2, color: "#ffc107" },
          },
          {
            name: "挑战二档",
            type: "line",
            smooth: true,
            data: challenge2Data,
            symbol: "circle",
            symbolSize: 4,
            itemStyle: {
              color: "#ff5252",
              borderColor: "#fff",
              borderWidth: 1,
            },
            lineStyle: { width: 2, color: "#ff5252" },
          },
          {
            name: "挑战一档",
            type: "line",
            smooth: true,
            data: challenge1Data,
            symbol: "circle",
            symbolSize: 4,
            itemStyle: {
              color: "#69f0ae",
              borderColor: "#fff",
              borderWidth: 1,
            },
            lineStyle: { width: 2, color: "#69f0ae" },
          },
        ],
      });

      // Initialize charts
      this.initReservesChart();
      this.initOilChart();
      this.initGasChart();
    },

    initReservesChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getFullYear() + "/" + (d.getMonth() + 1));
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "年");
          }
        }
        return result;
      };

      const createScoreChartOption = (
        timeMode,
        baseData,
        challenge2Data,
        challenge1Data
      ) => ({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          top: 0,
          right: 0,
          textStyle: { color: "#fff", fontSize: 8 },
          itemWidth: 8,
          itemHeight: 8,
        },
        grid: { top: 30, bottom: 20, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: generateTimeAxis(timeMode),
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
          axisLabel: { color: labelColor, fontSize: 8, rotate: 30 },
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          // 基本目标 - 平滑折线
          {
            name: "基本目标",
            type: "line",
            smooth: true,
            data: baseData,
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              color: "#ffc107",
              borderColor: "#fff",
              borderWidth: 2,
            },
            lineStyle: { width: 3, color: "#ffc107" },
          },
          // 挑战二档 - 平滑折线
          {
            name: "挑战二档",
            type: "line",
            smooth: true,
            data: challenge2Data,
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              color: "#ff5252",
              borderColor: "#fff",
              borderWidth: 2,
            },
            lineStyle: { width: 3, color: "#ff5252" },
          },
          // 挑战一档 - 平滑折线
          {
            name: "挑战一档",
            type: "line",
            smooth: true,
            data: challenge1Data,
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              color: "#69f0ae",
              borderColor: "#fff",
              borderWidth: 2,
            },
            lineStyle: { width: 3, color: "#69f0ae" },
            label: {
              show: true,
              position: "top",
              color: "#69f0ae",
              fontSize: 9,
              formatter: "{c}",
              fontWeight: "bold",
            },
          },
        ],
      });

      const chart = echarts.init(this.$refs.chartReserves);
      chart.setOption(
        createScoreChartOption(
          this.reservesTimeMode,
          [88, 88.5, 89, 89.2, 89.5, 89.8, 90, 90, 90, 90],
          [98, 98.5, 99, 99.2, 99.5, 99.8, 100, 100, 100, 100],
          [108, 108.5, 109, 109.2, 109.5, 109.8, 110, 110, 110, 110]
        )
      );
    },

    initOilChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getFullYear() + "/" + (d.getMonth() + 1));
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "年");
          }
        }
        return result;
      };

      const createScoreChartOption = (
        timeMode,
        baseData,
        challenge2Data,
        challenge1Data
      ) => ({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          top: 0,
          right: 0,
          textStyle: { color: "#fff", fontSize: 8 },
          itemWidth: 8,
          itemHeight: 8,
        },
        grid: { top: 30, bottom: 20, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: generateTimeAxis(timeMode),
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
          axisLabel: { color: labelColor, fontSize: 8, rotate: 30 },
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          // 基本目标 - 堆叠柱状图
          {
            name: "基本目标",
            type: "bar",
            stack: "total",
            barWidth: 18,
            data: baseData,
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(255, 193, 7, 0.9)" },
                { offset: 1, color: "rgba(255, 152, 0, 0.7)" },
              ]),
              borderRadius: [0, 0, 0, 0],
              shadowColor: "rgba(255, 193, 7, 0.8)",
              shadowBlur: 15,
              shadowOffsetY: 0,
            },
          },
          // 挑战二档 - 堆叠柱状图
          {
            name: "挑战二档",
            type: "bar",
            stack: "total",
            barWidth: 18,
            data: challenge2Data.map((val, idx) => val - baseData[idx]),
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(255, 82, 82, 0.9)" },
                { offset: 1, color: "rgba(211, 47, 47, 0.7)" },
              ]),
              borderRadius: [0, 0, 0, 0],
              shadowColor: "rgba(255, 82, 82, 0.8)",
              shadowBlur: 15,
              shadowOffsetY: 0,
            },
          },
          // 挑战一档 - 堆叠柱状图
          {
            name: "挑战一档",
            type: "bar",
            stack: "total",
            barWidth: 18,
            data: challenge1Data.map((val, idx) => val - challenge2Data[idx]),
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(105, 240, 174, 0.9)" },
                { offset: 1, color: "rgba(0, 200, 83, 0.7)" },
              ]),
              borderRadius: [4, 4, 0, 0],
              shadowColor: "rgba(105, 240, 174, 0.8)",
              shadowBlur: 15,
              shadowOffsetY: 0,
            },
            label: {
              show: true,
              position: "top",
              color: "#69f0ae",
              fontSize: 9,
              formatter: function (params) {
                return challenge1Data[params.dataIndex];
              },
              fontWeight: "bold",
            },
          },
        ],
      });

      const chart = echarts.init(this.$refs.chartOil);
      chart.setOption(
        createScoreChartOption(
          this.oilTimeMode,
          [87, 87.5, 88, 88.5, 89, 89.2, 89.5, 89.8, 90, 90],
          [97, 97.5, 98, 98.5, 99, 99.2, 99.5, 99.8, 100, 100],
          [107, 107.5, 108, 108.5, 109, 109.2, 109.5, 109.8, 110, 110]
        )
      );
    },

    initGasChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getFullYear() + "/" + (d.getMonth() + 1));
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "年");
          }
        }
        return result;
      };

      const createScoreChartOption = (
        timeMode,
        baseData,
        challenge2Data,
        challenge1Data
      ) => ({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          top: 0,
          right: 0,
          textStyle: { color: "#fff", fontSize: 8 },
          itemWidth: 8,
          itemHeight: 8,
        },
        grid: { top: 30, bottom: 20, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: generateTimeAxis(timeMode),
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
          axisLabel: { color: labelColor, fontSize: 8, rotate: 30 },
          boundaryGap: false,
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          // 基本目标 - 面积图
          {
            name: "基本目标",
            type: "line",
            smooth: true,
            data: baseData,
            symbol: "circle",
            symbolSize: 5,
            itemStyle: { color: "#ffc107" },
            lineStyle: { width: 2, color: "#ffc107" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(255, 193, 7, 0.5)" },
                { offset: 1, color: "rgba(255, 193, 7, 0.1)" },
              ]),
            },
          },
          // 挑战二档 - 面积图
          {
            name: "挑战二档",
            type: "line",
            smooth: true,
            data: challenge2Data,
            symbol: "circle",
            symbolSize: 5,
            itemStyle: { color: "#ff5252" },
            lineStyle: { width: 2, color: "#ff5252" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(255, 82, 82, 0.5)" },
                { offset: 1, color: "rgba(255, 82, 82, 0.1)" },
              ]),
            },
          },
          // 挑战一档 - 面积图
          {
            name: "挑战一档",
            type: "line",
            smooth: true,
            data: challenge1Data,
            symbol: "circle",
            symbolSize: 5,
            itemStyle: { color: "#69f0ae" },
            lineStyle: { width: 2, color: "#69f0ae" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(105, 240, 174, 0.5)" },
                { offset: 1, color: "rgba(105, 240, 174, 0.1)" },
              ]),
            },
            label: {
              show: true,
              position: "top",
              color: "#69f0ae",
              fontSize: 9,
              formatter: "{c}",
              fontWeight: "bold",
            },
          },
        ],
      });

      const chart = echarts.init(this.$refs.chartGas);
      chart.setOption(
        createScoreChartOption(
          this.gasTimeMode,
          [86, 86.5, 87, 87.5, 88, 88.5, 89, 89.5, 89.8, 90],
          [96, 96.5, 97, 97.5, 98, 98.5, 99, 99.5, 99.8, 100],
          [106, 106.5, 107, 107.5, 108, 108.5, 109, 109.5, 109.8, 110]
        )
      );
    },

    switchReservesTime(mode) {
      this.reservesTimeMode = mode;
      this.initReservesChart();
    },

    switchOilTime(mode) {
      this.oilTimeMode = mode;
      this.initOilChart();
    },

    switchGasTime(mode) {
      this.gasTimeMode = mode;
      this.initGasChart();
    },

    switchProfitTime(mode) {
      this.profitTimeMode = mode;
      this.initProfitChart();
    },

    switchBarrelTime(mode) {
      this.barrelTimeMode = mode;
      this.initBarrelChart();
    },

    switchRevenueTime(mode) {
      this.revenueTimeMode = mode;
      this.initRevenueChart();
    },

    switchInvestTime(mode) {
      this.investTimeMode = mode;
      this.initInvestChart();
    },

    initInvestChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getMonth() + 1 + "月");
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "");
          }
        }
        return result;
      };

      const timeData = generateTimeAxis(this.investTimeMode);

      const chart = echarts.init(this.$refs.chart3);
      chart.setOption({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          show: true,
          textStyle: { color: "#fff", fontSize: 8 },
          top: 0,
          right: 0,
          itemWidth: 10,
          itemHeight: 10,
        },
        grid: { top: 35, bottom: 25, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: timeData,
          axisLabel: { color: "#fff", fontSize: 9, interval: 0, rotate: 30 },
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
          boundaryGap: false,
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          {
            name: "基本目标",
            type: "line",
            smooth: true,
            data: [85, 85.5, 86, 86.5, 87, 87.5, 88, 88.5, 89, 90],
            symbol: "circle",
            symbolSize: 5,
            itemStyle: { color: "#ffc107" },
            lineStyle: { width: 2, color: "#ffc107" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(255, 193, 7, 0.4)" },
                { offset: 1, color: "rgba(255, 193, 7, 0.05)" },
              ]),
            },
          },
          {
            name: "挑战二档",
            type: "line",
            smooth: true,
            data: [95, 95.5, 96, 96.5, 97, 97.5, 98, 98.5, 99, 100],
            symbol: "circle",
            symbolSize: 5,
            itemStyle: { color: "#ff5252" },
            lineStyle: { width: 2, color: "#ff5252" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(255, 82, 82, 0.4)" },
                { offset: 1, color: "rgba(255, 82, 82, 0.05)" },
              ]),
            },
          },
          {
            name: "挑战一档",
            type: "line",
            smooth: true,
            data: [105, 105.5, 106, 106.5, 107, 107.5, 108, 108.5, 109, 110],
            symbol: "circle",
            symbolSize: 5,
            itemStyle: { color: "#69f0ae" },
            lineStyle: { width: 2, color: "#69f0ae" },
            areaStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "rgba(105, 240, 174, 0.4)" },
                { offset: 1, color: "rgba(105, 240, 174, 0.05)" },
              ]),
            },
            label: {
              show: true,
              position: "top",
              color: "#69f0ae",
              fontSize: 8,
              formatter: "{c}",
            },
          },
        ],
      });
    },

    initRevenueChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getMonth() + 1 + "月");
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "");
          }
        }
        return result;
      };

      const timeData = generateTimeAxis(this.revenueTimeMode);

      const chart = echarts.init(this.$refs.chart4);
      chart.setOption({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          show: true,
          textStyle: { color: "#fff", fontSize: 8 },
          top: 0,
          right: 0,
          itemWidth: 10,
          itemHeight: 10,
        },
        grid: { top: 35, bottom: 25, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: timeData,
          axisLabel: { color: "#fff", fontSize: 9, interval: 0, rotate: 30 },
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          {
            name: "基本目标",
            type: "scatter",
            data: [86, 86.5, 87, 87.5, 88, 88.5, 89, 89.5, 89.8, 90],
            symbolSize: 12,
            itemStyle: {
              color: new echarts.graphic.RadialGradient(0.5, 0.5, 1, [
                { offset: 0, color: "#ffc107" },
                { offset: 0.7, color: "#ff9800" },
                { offset: 1, color: "rgba(255, 193, 7, 0.3)" },
              ]),
              shadowBlur: 8,
              shadowColor: "#ffc107",
              borderColor: "#ffc107",
              borderWidth: 1,
            },
          },
          {
            name: "挑战二档",
            type: "scatter",
            data: [96, 96.5, 97, 97.5, 98, 98.5, 99, 99.5, 99.8, 100],
            symbolSize: 12,
            itemStyle: {
              color: new echarts.graphic.RadialGradient(0.5, 0.5, 1, [
                { offset: 0, color: "#ff5252" },
                { offset: 0.7, color: "#d32f2f" },
                { offset: 1, color: "rgba(255, 82, 82, 0.3)" },
              ]),
              shadowBlur: 8,
              shadowColor: "#ff5252",
              borderColor: "#ff5252",
              borderWidth: 1,
            },
          },
          {
            name: "挑战一档",
            type: "scatter",
            data: [106, 106.5, 107, 107.5, 108, 108.5, 109, 109.5, 109.8, 110],
            symbolSize: 12,
            itemStyle: {
              color: new echarts.graphic.RadialGradient(0.5, 0.5, 1, [
                { offset: 0, color: "#69f0ae" },
                { offset: 0.7, color: "#00c853" },
                { offset: 1, color: "rgba(105, 240, 174, 0.3)" },
              ]),
              shadowBlur: 10,
              shadowColor: "#69f0ae",
              borderColor: "#69f0ae",
              borderWidth: 1,
            },
            label: {
              show: true,
              position: "top",
              color: "#69f0ae",
              fontSize: 8,
              formatter: "{c}",
              distance: 5,
            },
          },
        ],
      });
    },

    initBarrelChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getMonth() + 1 + "月");
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "");
          }
        }
        return result;
      };

      const timeData = generateTimeAxis(this.barrelTimeMode);

      const chart = echarts.init(this.$refs.chart2);
      chart.setOption({
        tooltip: {
          trigger: "axis",
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          show: true,
          textStyle: { color: "#fff", fontSize: 8 },
          top: 0,
          right: 0,
          itemWidth: 10,
          itemHeight: 10,
        },
        grid: { top: 35, bottom: 25, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: timeData,
          axisLabel: { color: "#fff", fontSize: 9, interval: 0, rotate: 30 },
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          {
            name: "基本目标",
            type: "line",
            smooth: true,
            data: [87, 87.5, 88, 88.5, 89, 89.2, 89.5, 89.8, 90, 90],
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              color: "#ffc107",
              borderColor: "#fff",
              borderWidth: 2,
            },
            lineStyle: { width: 3, color: "#ffc107" },
          },
          {
            name: "挑战二档",
            type: "line",
            smooth: true,
            data: [97, 97.5, 98, 98.5, 99, 99.2, 99.5, 99.8, 100, 100],
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              color: "#ff5252",
              borderColor: "#fff",
              borderWidth: 2,
            },
            lineStyle: { width: 3, color: "#ff5252" },
          },
          {
            name: "挑战一档",
            type: "line",
            smooth: true,
            data: [107, 107.5, 108, 108.5, 109, 109.2, 109.5, 109.8, 110, 110],
            symbol: "circle",
            symbolSize: 6,
            itemStyle: {
              color: "#69f0ae",
              borderColor: "#fff",
              borderWidth: 2,
            },
            lineStyle: { width: 3, color: "#69f0ae" },
            label: {
              show: true,
              position: "top",
              color: "#69f0ae",
              fontSize: 8,
              formatter: "{c}",
            },
          },
        ],
      });
    },

    initProfitChart() {
      const echarts = require("echarts");
      const axisLineColor = "rgba(30, 90, 142, 0.5)";
      const splitLineColor = "rgba(30, 90, 142, 0.2)";
      const labelColor = "#6ea8d3";

      const generateTimeAxis = (mode) => {
        const result = [];
        const now = new Date();
        if (mode === "day") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setDate(d.getDate() - i);
            result.push(d.getMonth() + 1 + "/" + d.getDate());
          }
        } else if (mode === "month") {
          for (let i = 9; i >= 0; i--) {
            const d = new Date(now);
            d.setMonth(d.getMonth() - i);
            result.push(d.getMonth() + 1 + "月");
          }
        } else if (mode === "year") {
          for (let i = 9; i >= 0; i--) {
            result.push(now.getFullYear() - i + "");
          }
        }
        return result;
      };

      const timeData = generateTimeAxis(this.profitTimeMode);

      const chart = echarts.init(this.$refs.chart1);
      chart.setOption({
        tooltip: {
          trigger: "axis",
          axisPointer: { type: "shadow" },
          formatter: function (params) {
            let result = params[0].axisValue + "<br/>";
            params.forEach((item) => {
              result +=
                item.marker +
                " " +
                item.seriesName +
                ": " +
                item.value +
                "分<br/>";
            });
            return result;
          },
        },
        legend: {
          show: true,
          textStyle: { color: "#fff", fontSize: 8 },
          top: 0,
          right: 0,
          itemWidth: 10,
          itemHeight: 10,
        },
        grid: { top: 35, bottom: 25, left: 10, right: 10, containLabel: true },
        xAxis: {
          type: "category",
          data: timeData,
          axisLabel: { color: "#fff", fontSize: 9, interval: 0, rotate: 30 },
          axisLine: { lineStyle: { color: axisLineColor } },
          axisTick: { show: false },
        },
        yAxis: {
          type: "value",
          min: 80,
          max: 115,
          interval: 10,
          splitLine: { lineStyle: { color: splitLineColor, type: "dashed" } },
          axisLabel: {
            color: labelColor,
            fontSize: 9,
            formatter: "{value}分",
          },
        },
        series: [
          {
            name: "基本目标",
            type: "bar",
            barWidth: 8,
            data: [88, 88.5, 89, 89.2, 89.5, 89.8, 90, 90, 90, 90],
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "#ffc107" },
                { offset: 1, color: "#ff9800" },
              ]),
            },
          },
          {
            name: "挑战二档",
            type: "bar",
            barWidth: 8,
            data: [98, 98.5, 99, 99.2, 99.5, 99.8, 100, 100, 100, 100],
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "#ff5252" },
                { offset: 1, color: "#d32f2f" },
              ]),
            },
          },
          {
            name: "挑战一档",
            type: "bar",
            barWidth: 8,
            data: [108, 108.5, 109, 109.2, 109.5, 109.8, 110, 110, 110, 110],
            itemStyle: {
              color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                { offset: 0, color: "#69f0ae" },
                { offset: 1, color: "#00c853" },
              ]),
            },
            label: {
              show: true,
              position: "top",
              color: "#fff",
              fontSize: 8,
              formatter: "{c}",
            },
          },
        ],
      });
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap");

/* Animations */
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
@keyframes pulse {
  0% {
    opacity: 0.6;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0.6;
  }
}
@keyframes scanline {
  0% {
    top: -100%;
  }
  100% {
    top: 200%;
  }
}

/* Global Layout */
.dashboard-container {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(180deg, #0a1929 0%, #0d1b2a 50%, #0a1520 100%);
  color: #fff;
  font-family: "Microsoft YaHei", "PingFang SC", sans-serif;
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
  background: linear-gradient(
    180deg,
    rgba(10, 25, 41, 0.95) 0%,
    rgba(13, 27, 42, 0.9) 100%
  );
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
  font-family: "Arial", sans-serif;
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
  font-family: "Consolas", "Courier New", monospace;
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
  background: linear-gradient(
    135deg,
    rgba(10, 35, 60, 0.7) 0%,
    rgba(10, 25, 45, 0.6) 100%
  );
  border: 1px solid rgba(30, 90, 142, 0.4);
  position: relative;
  padding: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(8px);
}
/* Corner Accents */
.tech-box::before {
  content: "";
  position: absolute;
  top: -1px;
  left: -1px;
  width: 12px;
  height: 12px;
  border-top: 2px solid #00d4ff;
  border-left: 2px solid #00d4ff;
}
.tech-box::after {
  content: "";
  position: absolute;
  bottom: -1px;
  right: -1px;
  width: 12px;
  height: 12px;
  border-bottom: 2px solid #00d4ff;
  border-right: 2px solid #00d4ff;
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
.tech-box-title .icon {
  margin-right: 6px;
  font-size: 10px;
  color: #00d4ff;
}

/* User Box */
.user-info-box {
  height: 85px;
  display: flex;
  flex-direction: column;
}
.user-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex: 1;
  padding: 0 5px;
}

.avatar-section {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.avatar-ring {
  width: 42px;
  height: 42px;
  border-radius: 50%;
  border: 2px solid #00d4ff;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: spin 10s linear infinite;
  box-shadow: 0 0 10px rgba(0, 212, 255, 0.5);
}
.avatar-inner {
  width: 32px;
  height: 32px;
  background: linear-gradient(135deg, #00d4ff, #0088ff);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #000;
  font-size: 16px;
  animation: spin 10s linear infinite reverse;
}
.status-badge {
  display: inline-block;
  background: rgba(0, 255, 0, 0.2);
  color: #0f0;
  border: 1px solid #0f0;
  font-size: 9px;
  padding: 0 5px;
  border-radius: 2px;
}

.user-details-grid {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 5px 10px;
  align-content: center;
}
.user-details-list {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 1px;
  justify-content: center;
}
.detail-row {
  display: flex;
  align-items: center;
  font-size: 10px;
  border-bottom: 1px dashed rgba(30, 90, 142, 0.3);
  padding-bottom: 2px;
}
.detail-row .label {
  color: #6ea8d3;
  width: 30px;
  text-align: right;
  margin-right: 6px;
  font-size: 10px;
}
.detail-row .value {
  color: #fff;
  font-weight: bold;
  font-size: 10px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.tech-font {
  font-family: "Consolas", monospace;
  color: #00d4ff !important;
}

.action-grid {
  display: flex;
  gap: 5px;
  margin-top: 5px;
}
.action-item {
  flex: 1;
  text-align: center;
  background: rgba(30, 90, 142, 0.2);
  font-size: 12px;
  padding: 6px 0;
  cursor: pointer;
  border: 1px solid rgba(30, 90, 142, 0.3);
  transition: all 0.3s;
}
.action-item:hover,
.action-item.active {
  border-color: #00d4ff;
  color: #00d4ff;
  background: rgba(0, 212, 255, 0.2);
  box-shadow: 0 0 8px rgba(0, 212, 255, 0.3);
}

/* Left Charts List */
.full-height-box {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.chart-column {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 5px;
  overflow-y: auto;
}
.sub-chart-box {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: linear-gradient(90deg, rgba(30, 90, 142, 0.1), transparent);
  padding: 6px;
  border-left: 3px solid rgba(0, 212, 255, 0.4);
}
.sub-title {
  font-size: 12px;
  color: #6ea8d3;
  margin-bottom: 3px;
}
.chart-ref {
  flex: 1;
  width: 100%;
  min-height: 50px;
}

/* Right Content */
.right-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

/* Top Stat Cards */
.top-cards-row {
  display: flex;
  gap: 10px;
  height: 80px;
}
.stat-card {
  flex: 1;
  position: relative;
  background: linear-gradient(
    135deg,
    rgba(10, 40, 70, 0.7),
    rgba(10, 30, 55, 0.6)
  );
  border: 1px solid rgba(30, 90, 142, 0.4);
  display: flex;
  align-items: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}
.stat-card-inner {
  display: flex;
  align-items: center;
  padding: 10px;
  width: 100%;
}
.card-icon-box {
  width: 40px;
  height: 40px;
  background: linear-gradient(
    135deg,
    rgba(0, 212, 255, 0.2),
    rgba(0, 136, 255, 0.1)
  );
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: #00d4ff;
  margin-right: 10px;
  clip-path: polygon(
    10px 0,
    100% 0,
    100% calc(100% - 10px),
    calc(100% - 10px) 100%,
    0 100%,
    0 10px
  );
  box-shadow: 0 0 10px rgba(0, 212, 255, 0.3);
}
.stat-title {
  font-size: 12px;
  color: #6ea8d3;
}
.stat-item {
  font-size: 11px;
  color: #fff;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100px;
}

.corner-decor {
  position: absolute;
  width: 10px;
  height: 10px;
  border-color: #00d4ff;
  border-style: solid;
}
.corner-decor.lt {
  top: 0;
  left: 0;
  border-width: 2px 0 0 2px;
}
.corner-decor.rb {
  bottom: 0;
  right: 0;
  border-width: 0 2px 2px 0;
}

/* Main Center Area */
.charts-and-extras {
  flex: 1;
  display: flex;
  gap: 10px;
}

.center-area {
  flex: 3;
  display: flex;
  flex-direction: column;
  position: relative;
}

.map-border-box {
  flex: 1;
  border: 1px solid rgba(30, 90, 142, 0.4);
  background: linear-gradient(
    135deg,
    rgba(10, 30, 50, 0.6),
    rgba(10, 25, 40, 0.5)
  );
  position: relative;
  display: flex;
  flex-direction: column;
  padding: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
}
/* Complex Center Border Animation */
.border-line {
  position: absolute;
  height: 2px;
  background: #00d4ff;
  width: 30%;
  transition: all 0.5s;
  box-shadow: 0 0 8px #00d4ff;
}
.top-line {
  top: 0;
  left: 35%;
}
.bottom-line {
  bottom: 0;
  left: 35%;
}

.filter-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.section-label {
  font-family: "Arial";
  color: #00d4ff;
  font-size: 16px;
  letter-spacing: 2px;
  font-weight: bold;
}
.time-tabs {
  display: flex;
  gap: 5px;
}
.tab {
  padding: 4px 12px;
  font-size: 11px;
  background: rgba(30, 90, 142, 0.3);
  border: 1px solid rgba(30, 90, 142, 0.5);
  cursor: pointer;
  color: #6ea8d3;
  transition: all 0.3s;
}
.tab.active {
  border-color: #00d4ff;
  color: #00d4ff;
  background: rgba(0, 212, 255, 0.2);
  box-shadow: 0 0 8px rgba(0, 212, 255, 0.4);
}

.center-grid {
  flex: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 15px;
}
.grid-cell {
  background: linear-gradient(
    135deg,
    rgba(10, 40, 70, 0.15),
    rgba(10, 30, 50, 0.1)
  );
  border: 1px solid rgba(30, 90, 142, 0.3);
  display: flex;
  flex-direction: column;
  padding: 10px;
}
.cell-header-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
}
.cell-title {
  font-size: 14px;
  color: #fff;
  border-left: 3px solid #00d4ff;
  padding-left: 8px;
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
.cell-chart {
  flex: 1;
  width: 100%;
  min-height: 0;
}

/* Right Sidebar */
.right-sidebar {
  width: 25%;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.extra-box {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.right-chart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgba(30, 90, 142, 0.5);
  padding-bottom: 6px;
  margin-bottom: 10px;
}

.right-tabs {
  display: flex;
  gap: 3px;
}

.r-tab {
  font-size: 10px;
  padding: 2px 8px;
  background: rgba(30, 90, 142, 0.2);
  border: 1px solid rgba(30, 90, 142, 0.4);
  color: #6ea8d3;
  cursor: pointer;
  border-radius: 2px;
  transition: all 0.3s;
}

.r-tab:hover {
  border-color: #00d4ff;
  color: #00d4ff;
}

.r-tab.active {
  background: rgba(0, 212, 255, 0.25);
  border-color: #00d4ff;
  color: #00d4ff;
  box-shadow: 0 0 6px rgba(0, 212, 255, 0.4);
}

.news-scroll {
  flex: 1;
  overflow-y: auto;
  padding: 5px;
}
.news-row {
  font-size: 11px;
  margin-bottom: 8px;
  border-bottom: 1px dashed rgba(30, 90, 142, 0.3);
  padding-bottom: 4px;
  display: flex;
}
.news-time {
  color: #00d4ff;
  margin-right: 8px;
  font-family: "Consolas", monospace;
}
.news-text {
  color: #b8c9d8;
}

.monitor-list {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  flex: 1;
  padding: 5px;
}
.monitor-item {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 11px;
}
.label {
  width: 60px;
  color: #6ea8d3;
}
.bar-bg {
  flex: 1;
  height: 8px;
  background: rgba(30, 90, 142, 0.3);
  border-radius: 4px;
  overflow: hidden;
}
.bar-fill {
  height: 100%;
  box-shadow: 0 0 8px currentColor;
  transition: width 0.5s;
}
.val {
  width: 30px;
  text-align: right;
  color: #fff;
  font-family: "Consolas", monospace;
  font-weight: bold;
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
  background: rgba(0, 212, 255, 0.5);
  border-radius: 2px;
}
::-webkit-scrollbar-thumb:hover {
  background: #00d4ff;
}
</style>
