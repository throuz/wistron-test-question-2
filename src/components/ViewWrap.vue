<template>
  <v-treeview :items="items" item-key="name" open-on-click>
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
  }),

  created() {
    const treeData = [];
    // Reorganize MainGroup
    const groupFirst = [...new Set(MainGroup.items.map((item) => item[0]))];
    groupFirst.forEach((el) => {
      treeData.push({ name: el });
    });
    MainGroup.items.forEach((item) => {
      treeData.forEach((el) => {
        if (el.name === item[0]) {
          if (el.children) {
            el.children.push({ name: item[1] });
          } else {
            el.children = [{ name: item[1] }];
          }
        }
      });
    });
    // Reorganize SubGroup
    SubGroup.items.forEach((item) => {
      treeData.forEach((node) => {
        node.children &&
          node.children.forEach((el) => {
            if (el.name === item[1]) {
              if (el.children) {
                el.children.push({ name: item[0] });
              } else {
                el.children = [{ name: item[0] }];
              }
            }
            el.children &&
              el.children.forEach((element) => {
                if (element.name === item[1]) {
                  if (element.children) {
                    element.children.push({ name: item[0] });
                  } else {
                    element.children = [{ name: item[0] }];
                  }
                }
              });
          });
      });
    });
    this.items = treeData;
  },
};
</script>
