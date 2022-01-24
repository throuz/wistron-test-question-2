<template>
  <v-treeview
    :items="items"
    item-key="name"
    open-on-click
    @update:open="addSubGroup"
  >
    <template v-slot:prepend="{ open }">
      <v-icon>
        {{ open ? "mdi-folder-open" : "mdi-folder" }}
      </v-icon>
    </template>
  </v-treeview>
</template>

<script>
import { MainGroup, SubGroup } from "@/mock/tree.js";

export default {
  name: "ViewWrap",

  data: () => ({
    items: [],
    firstTrigger: true,
    isAddSub: false,
  }),

  created() {
    // Reorganize MainGroup
    const treeData = [];
    const groupFirst = [...new Set(MainGroup.items.map((item) => item[0]))];
    groupFirst.forEach((el) => {
      treeData.push({ name: el, children: [] });
    });
    MainGroup.items.forEach((item) => {
      treeData.forEach((nodeFirst) => {
        this.addNode(nodeFirst, item[0], item[1]);
      });
    });

    this.items = treeData;
  },

  mounted() {
    this.firstTrigger = false;
  },

  methods: {
    addNode(parentNode, name, childrenName) {
      if (parentNode.name === name) {
        parentNode.children.push({ name: childrenName, children: [] });
      }
    },
    addSubGroup() {
      // Because @update:open is triggered when mounting, it is necessary to prevent the first rendering
      if (!this.firstTrigger && !this.isAddSub) {
        // Reorganize SubGroup
        SubGroup.items.forEach((item) => {
          this.items.forEach((nodeFirst) => {
            nodeFirst.children.forEach((nodeSecond) => {
              this.addNode(nodeSecond, item[1], item[0]);
              nodeSecond.children.forEach((nodeThird) => {
                this.addNode(nodeThird, item[1], item[0]);
              });
            });
          });
        });
        this.isAddSub = true;
      }
    },
  },
};
</script>
