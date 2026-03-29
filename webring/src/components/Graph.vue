<script setup lang="ts">
import { markRaw, reactive } from "vue";
import * as vNG from "v-network-graph";
import {
  ForceLayout,
  type ForceEdgeDatum,
  type ForceNodeDatum,
} from "v-network-graph/lib/force-layout";
import "v-network-graph/lib/style.css";
import membersArray from "../../../members.json";
import type { Member } from "../types";
import type { Layouts } from "v-network-graph";

const members: Member[] = membersArray as Member[];
const nodes = reactive({});
const edges = reactive({});
const layout = reactive<Layouts>({ nodes: {} });

buildNetwork(nodes, edges);

const layoutHandler = markRaw(
  new ForceLayout({
    positionFixedByDrag: false,
    createSimulation: (d3, nodes, edges) => {
      const forceLink = d3
        .forceLink<ForceNodeDatum, ForceEdgeDatum>(edges)
        .id((d: { id: any }) => d.id)
        .distance(10) // increase to push linked nodes further apart
        .strength(0.1); // lower = links are less rigid

      return d3
        .forceSimulation(nodes)
        .force("edge", forceLink)
        .force("charge", d3.forceManyBody().strength(-700)) // more negative = stronger repulsion between nodes
        .force("center", d3.forceCenter().strength(0.06)) // lower = weaker pull toward center
        .alphaMin(0.001);
    },
  }),
);

const configs = reactive(
  vNG.defineConfigs({
    view: {
      layoutHandler,
    },
    node: {
      label: {
        visible: true,
      },
    },
  }),
);

function buildNetwork(nodes: vNG.Nodes, edges: vNG.Edges) {
  const count = members.length;

  const newNodes = Object.fromEntries(
    members.map((member, index) => [`node${index}`, { ...member }]),
  );

  Object.keys(nodes).forEach((id) => delete nodes[id]);
  Object.assign(nodes, newNodes);

  const makeEdgeEntry = (id1: number, id2: number) => {
    return [
      `edge${id1}-${id2}`,
      { source: `node${id1}`, target: `node${id2}` },
    ];
  };

  const newEdges = Object.fromEntries(
    members.map((_, index) => {
      const targetIndex = (index + 1) % count;
      return makeEdgeEntry(index, targetIndex);
    }),
  );

  layout.nodes = {};
  for (let i = 0; i < members.length; i++) {
    const randomX = Math.floor(Math.random() * 600) - 300;
    const randomY = Math.floor(Math.random() * 400) - 200;
    layout.nodes[`node${i}`] = { x: randomX, y: randomY };
  }

  Object.keys(edges).forEach((id) => delete edges[id]);
  Object.assign(edges, newEdges);
}

const eventHandlers: EventHandlers = {
  "node:click": ({ node, event }) => {
    if (event.ctrlKey) {
      // ...
    }
  },
};
</script>

<template>
  <v-network-graph
    class="graph"
    :zoom-level="0.5"
    :nodes="nodes"
    :edges="edges"
    :layouts="layout"
    :configs="configs"
  />
</template>

<style scoped>
.graph {
  width: 800px;
  height: 600px;
  border: 1px solid #cccccc;
  background-color: #333333;
}
</style>
