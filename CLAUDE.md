# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Vue.js 2 data visualization dashboard project inspired by the Haineng BMS (Business Management System) design. The dashboard features a deep blue sci-fi themed interface with ECharts-based visualizations.

## Development Commands

### Working Directory
All npm commands should be run from the `dashboard/` directory:
```bash
cd dashboard
```

### Development Server
```bash
npm run serve
```
Starts development server with hot-reload at http://localhost:8082/

### Production Build
```bash
npm run build
```
Compiles and minifies for production deployment

### Linting
```bash
npm run lint
```
Runs ESLint to check and fix code style issues

## Architecture

### Technology Stack
- **Vue 2.6.14**: Core framework
- **Element UI 2.15.14**: UI component library
- **ECharts 6.0.0**: Data visualization library
- **Vue CLI 5.0**: Build tooling

### Project Structure
```
dashboard/
├── src/
│   ├── App.vue              # Root component
│   ├── main.js              # Application entry point
│   ├── components/
│   │   ├── Dashboard.vue    # Main dashboard component (active)
│   │   └── EnhancedDashboard.vue  # Alternative BMS-style dashboard
│   └── assets/
├── public/
└── vue.config.js            # Vue CLI configuration (lintOnSave: false)
```

### Main Dashboard Component (Dashboard.vue)

The Dashboard.vue component is a single-file component containing:

1. **Layout Structure** (3-column):
   - Left Sidebar (25%): User info, production monitoring charts
   - Center Area (50%): 2x2 grid of metric charts with tabs
   - Right Sidebar (25%): Gauge chart, real-time news, system monitoring

2. **Chart Configuration**:
   - All charts use ECharts with custom configurations
   - Common axis colors: `#1e5a8e` (borders), `#6ea8d3` (labels)
   - Chart types: bar, line, pie, donut, radar, gauge, pictorial bar
   - Gradients use `echarts.graphic.LinearGradient` helper

3. **Design System Colors**:
   - Background: `linear-gradient(180deg, #0a1929, #0d1b2a, #0a1520)`
   - Primary accent: `#00d4ff` (cyan-blue)
   - Secondary accent: `#ff9800` (orange)
   - Card background: `rgba(10, 35, 60, 0.7)` with blur backdrop
   - Border primary: `#1e5a8e` (navy blue)
   - Text primary: `#ffffff`, secondary: `#6ea8d3`

4. **Component Styling Patterns**:
   - `.tech-box`: Standard card container with corner accents (::before/::after)
   - `.corner-decor`: 10px corner borders on stat cards
   - All interactive elements have hover states with `#00d4ff` glow
   - Scrollbars customized with 4px width and cyan-blue theme

### State Management
No Vuex or external state management - all data is local to components. Charts are initialized in `mounted()` hook and resized on window resize events.

### Chart Initialization Pattern
```javascript
mounted() {
  this.updateTime();
  setInterval(this.updateTime, 1000);
  this.initCharts();
  window.addEventListener('resize', this.handleResize);
}

handleResize() {
  // Get chart instance and call resize()
  echarts.getInstanceByDom(this.$refs.chartRef).resize();
}
```

## Design Guidelines

### Color Palette (BMS Style)
- Use `#00d4ff` for primary highlights, borders, and data emphasis
- Use `#1e5a8e` for structural borders and subtle separators
- Use `#ff9800` for secondary data series and warnings
- Use `#6ea8d3` for labels and secondary text
- Background gradients should transition between `#0a1929` and `#0d1b2a`

### Component Patterns
- All major containers use `.tech-box` with corner accent decorators
- Cards should have `backdrop-filter: blur(8px)` for depth
- Interactive elements transition on `:hover` with `box-shadow` glow effects
- Use `border-radius: 4px` for most containers (cards, buttons)

### Typography
- Headers: White (`#ffffff`) with gradient fills for titles
- Data values: Monospace font (`Consolas`) with cyan accent
- Labels: `#6ea8d3` at 11-12px
- Time display: `Consolas` monospace with cyan glow effect

## ECharts Configuration Notes

### Common Patterns
- Grid spacing: `{ top: 25, bottom: 5, left: 0, right: 0, containLabel: true }`
- Axis lines: `rgba(30, 90, 142, 0.5)` solid color
- Split lines: `rgba(30, 90, 142, 0.2)` dashed
- Legend text: `{ color: '#6ea8d3', fontSize: 9-11 }`

### Gradient Helper
Use the existing helper function for consistent gradients:
```javascript
const getLinearGradient = (c1, c2) =>
  new echarts.graphic.LinearGradient(0, 0, 0, 1, [
    {offset: 0, color: c1},
    {offset: 1, color: c2}
  ]);
```

### Chart Refs
Charts are bound to template refs and initialized in `initCharts()`:
```javascript
const chart = echarts.init(this.$refs.chartName);
chart.setOption({ /* config */ });
```

## Element UI Configuration

Element UI is registered globally in `main.js`:
```javascript
import ElementUI from 'element-ui';
Vue.use(ElementUI);
```

Custom theming overrides are in Dashboard.vue scoped styles using deep selectors (`>>>`).
