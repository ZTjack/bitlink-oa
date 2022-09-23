<template>
  <div class="home">
    <el-menu @select="handleSelect">
      <el-submenu
        v-for="config in configs"
        :key="config.name"
        :index="config.name"
      >
        <template slot="title">
          <span> {{ config.name }}</span>
          <el-menu-item
            v-for="graph in config.graphs"
            :key="graph.graph"
            :index="graph.graph"
          >
            {{ graph.graph }}
          </el-menu-item>
        </template>
      </el-submenu>
    </el-menu>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
@Component({
  components: {
    HelloWorld,
  },
})
export default class HomeView extends Vue {
  private graphPrefix = "//grafana.qbtrade.org/dashboard/db/";

  private form = {
    graph: "stra-dionysus-template",
    params: "var-stra=strategy%2Fzop-mm1-btc-usd&refresh=1m",
    timeRange: "24h",
  };

  // `${this.graphPrefix}/${graph}?${form.params}&from=now-${form.timeRange}&to=now`

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

  private handleSelect(key: string, selects: string[]) {
    console.log("key", key);
    console.log("selects", selects);
  }
}
</script>
