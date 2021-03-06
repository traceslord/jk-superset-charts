# jk-superset-charts

<p>
  <img alt="JK" src="https://img.shields.io/badge/-JK-brightgreen">
  <a href="https://github.com/traceslord/jk-superset-charts">
    <img alt="code size" src="https://img.shields.io/github/languages/code-size/traceslord/jk-superset-charts">
  </a>
  <img alt="version" src="https://img.shields.io/github/package-json/v/traceslord/jk-superset-charts">
  <a href="https://github.com/traceslord/jk-superset-charts/blob/master/LICENSE">
    <img src="https://img.shields.io/github/license/traceslord/jk-superset-charts" alt="license">
  </a>
</p>

> Superset Charts in JK.

## Usage

前往`jk-superset-charts/service/api.js`更改服务器主机地址与端口为 superset 所部署的地址与端口。

```js
const PREFIX = "http://127.0.0.1:8088/api/v1";
```

在路由的全局前置守卫里通过登录请求获取 superset 的 token，并存入本地存储里。
其中`jk_superset_token`可以是任何`string`，可配合`jk-superset-charts/service/axios.js`进行同步更改。
配置`params`中的`username`和`password`为 superset 中所注册的用户名与密码。

```js
import supersetService from "jk-superset-charts/service";

// ...
router.beforeEach(async (to, from, next) => {
  // ...
  try {
    const SUPERSETTOKEN = localStorage.getItem("jk_superset_token");
    if (!SUPERSETTOKEN) {
      const params = {
        username: "admin",
        password: "123456",
        provider: "db",
        refresh: true
      };
      const superset = await supersetService.securityLogin(params);
      localStorage.setItem("jk_superset_token", superset.access_token);
    }
    // ...
    next();
  } catch {
    next();
  }
});
// ...
```

然后通过组件注册使用它，数据可通过`service`获取，示例如下：

```html
<template v-if="chart.type === 'jk_echarts_bar'">
  <jk-bar
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-bar>
</template>

<template v-if="chart.type === 'jk_echarts_diy'">
  <jk-echarts-diy
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-echarts-diy>
</template>

<template v-if="chart.type === 'jk_echarts_gantt'">
  <jk-gantt
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-gantt>
</template>

<template v-if="chart.type === 'jk_echarts_hydrograph'">
  <jk-hydrograph
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-hydrograph>
</template>

<template v-if="chart.type === 'jk_echarts_line'">
  <jk-line
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-line>
</template>

<template v-if="chart.type === 'jk_echarts_line_bar'">
  <jk-line-bar
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-line-bar>
</template>

<template v-if="chart.type === 'jk_number'">
  <jk-number
    :chart-id="chart.chart_id"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-number>
</template>

<template v-if="chart.type === 'jk_echarts_pie'">
  <jk-pie
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-pie>
</template>

<template v-if="chart.type === 'jk_echarts_sankey'">
  <jk-sankey
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-sankey>
</template>

<template v-if="chart.type === 'jk_echarts_scatter'">
  <jk-scatter
    :id="chart.id"
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-scatter>
</template>

<template v-if="chart.type === 'table'">
  <jk-table
    :chart-id="chart.chart_id"
    :chart-name="chart.name"
    :chart-description="chart.description"
    :chart-config="chart.config"
    :chart-data="chart.data"
    :chart-colnames="chart.colnames"
    :width="chart.width"
    :height="chart.height"
    is-skip
    is-export
  >
    <template v-slot:skip></template>
    <template v-slot:export></template>
  </jk-table>
</template>
```

```js
import supersetService from "jk-superset-charts/service";
import {
  JkBar,
  JkEchartsDiy,
  JkGantt,
  JkHydrograph,
  JkLine,
  JkLineBar,
  JkNumber,
  JkPie,
  JkSankey,
  JkScatter,
  JkTable
} from "jk-superset-charts";

export default {
  components: {
    JkBar,
    JkEchartsDiy,
    JkGantt,
    JkHydrograph,
    JkLine,
    JkLineBar,
    JkNumber,
    JkPie,
    JkSankey,
    JkScatter,
    JkTable
  },
  data() {
    return {
      chart: {
        id: 1,
        chart_id: 1,
        width: 600,
        height: 400
      }
    };
  },
  created() {
    const id = this.chart.chart_id;
    supersetService.getData(id).then(res => {
      this.$set(chart, "type", res[2].viz_type);
      this.$set(chart, "name", res[0]);
      this.$set(data, "description", res[1]);
      this.$set(chart, "data", res[3].result[0].data);
      this.$set(chart, "config", res[2]);
      if (res[2].viz_type === "table") {
        this.$set(chart, "colnames", res[3].result[0].colnames);
      }
    });
  }
};
```

## License
[MIT](https://github.com/traceslord/jk-superset-charts/blob/master/LICENSE)

Copyright (c) 2021-present zhuhuajian
