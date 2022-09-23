<template>
  <div class="home">
    <div class="sidebar-area">
      <el-menu @select="handleSelect">
        <el-submenu
          v-for="config in configs"
          :key="config.name"
          :index="config.name"
        >
          <template #title>
            <span> {{ config.name }}</span>
          </template>
          <el-menu-item-group v-for="graph in config.graphs" :key="graph.graph">
            <template #title>{{ graph.graph }}</template>
            <el-menu-item
              v-for="shortItem in graph.shortcuts"
              :key="shortItem.params"
              :index="`${shortItem.params}---${graph.graph}`"
            >
              {{ shortItem.label }}
            </el-menu-item>
          </el-menu-item-group>
        </el-submenu>
      </el-menu>
    </div>
    <el-divider direction="vertical" class="divider-vertical-style" />
    <div class="operation-area">
      <div class="url-label-area">
        <div class="label-style">QueryURL:</div>
        <el-input v-model="mainPath" class="path-input-style" size="mini" />
        <el-button
          type="primary"
          class="search-btn-style"
          size="mini"
          @click="searchUrl"
        >
          Search
        </el-button>
      </div>

      <div class="form-area">
        <div class="label-style">Graph:</div>
        <el-input v-model="graph" size="mini" />
        <div class="label-style">Params:</div>
        <el-input v-model="formParams" size="mini" />
        <div class="label-style">Time:</div>
        <el-input v-model="timeRange" size="mini" />
      </div>

      <el-divider class="horizontal-divider-style" />

      <iframe :src="realUsePath" class="grafana-area" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({
  name: "HomeView",
})
export default class HomeView extends Vue {
  private graphPrefix = "//grafana.qbtrade.org/dashboard/db/";

  //grafana的主路径
  private mainPath = "";

  //graph值
  private graph = "";

  //参数值
  private formParams = "";

  //时间值
  private timeRange = "24h";

  //iframe实际使用的URL
  private realUsePath = "";

  private configs = [
    {
      name: "lwq",
      graphs: [
        {
          graph: "stra-dionysus-template",
          shortcuts: [
            {
              params: "var-stra=strategy%2Fzop-mm1-btc-usd&refresh=1m",
              label: "BTC-USD",
            },
            {
              params: "var-stra=strategy%2Fzop-mm1-eth-usd&refresh=1m",
              label: "ETH-USD",
            },
          ],
        },
        {
          graph: "stra-dionysus-template2",
          shortcuts: [
            {
              params: "var-stra=strategy%2Fzop-mm1-btc-usd&refresh=1m",
              label: "BTC-USD2",
            },
            {
              params: "var-stra=strategy%2Fzop-mm1-eth-usd&refresh=1m",
              label: "ETH-USD2",
            },
          ],
        },
      ],
    },
  ];

  //点击选择某一项
  private handleSelect(key: string) {
    const [params, graph] = key.split("---");
    this.graph = graph;
    this.formParams = params;
    this.changeUrl();
  }

  private changeUrl() {
    this.mainPath = `${this.graphPrefix}/${this.graph}?${this.formParams}&from=now-${this.timeRange}&to=now`;
  }

  private searchUrl() {
    this.realUsePath = this.mainPath;
  }
}
</script>

<style lang="scss" scoped>
.home {
  display: flex;
}

.sidebar-area {
  width: 10%;
}

.operation-area {
  margin-top: 10px;
  width: 100%;

  .url-label-area {
    display: flex;
    align-items: center;
  }

  .path-input-style {
    width: 100%;
    margin-right: 10px;
  }

  .search-btn-style {
    margin-right: 10px;
  }
}

.label-style {
  margin: 0 10px;
  height: 40px;
  line-height: 40px;
}

.divider-vertical-style {
  height: 100vh;
}

.form-area {
  margin-top: 10px;
  display: flex;
  margin-right: 10px;
  align-items: center;
}

.grafana-area {
  width: 100%;
  height: 100%;
}

.horizontal-divider-style {
  margin: 0;
}
</style>
