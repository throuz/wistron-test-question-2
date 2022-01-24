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
    const groupFirst = [...new Set(MainGroup.items.map((item) => item[0]))];
    groupFirst.forEach((el) => {
      this.items.push({ name: el, children: [] });
    });

    MainGroup.items.forEach((item) => {
      this.addSub(this.items, item[0], item[1]);
    });
  },

  mounted() {
    this.firstTrigger = false;
  },

  methods: {
    addSubGroup() {
      // Because @update:open is triggered when mounting, it is necessary to prevent the first rendering
      if (!this.firstTrigger && !this.isAddSub) {
        SubGroup.items.forEach((item) => {
          this.addSub(this.items, item[1], item[0]);
        });
        this.isAddSub = true;
      }
    },
    addSub(items, name, nodeName) {
      items.forEach((item) => {
        if (item.name === name) {
          item.children.push({ name: nodeName, children: [] });
          return;
        }
        item.children && this.addSub(item.children, name, nodeName);
      });
    },
  },
};
</script>
