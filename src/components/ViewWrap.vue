<template>
  <v-treeview
    v-model="tree"
    :items="items"
    activatable
    item-key="name"
    open-on-click
  >
    <template v-slot:prepend="{ item, open }">
      <v-icon v-if="!item.file">
        {{ open ? "mdi-folder-open" : "mdi-folder" }}
      </v-icon>
      <v-icon v-else>
        {{ files[item.file] }}
      </v-icon>
    </template>
  </v-treeview>
</template>

<script>
import { MainGroup, SubGroup } from "@/mock/tree.js";

export default {
  name: "ViewWrap",

  data: () => ({
    files: {
      html: "mdi-language-html5",
      js: "mdi-nodejs",
      json: "mdi-code-json",
      md: "mdi-language-markdown",
      pdf: "mdi-file-pdf",
      png: "mdi-file-image",
      txt: "mdi-file-document-outline",
      xls: "mdi-file-excel",
    },
    tree: [],
    items: [
      {
        name: "GroupAA",
        children: [
          {
            name: "Prod1",
            children: [
              {
                name: "Prod1_1",
                file: "png",
              },
              {
                name: "Prod1_2",
                children: [
                  {
                    name: "Prod1_2_1",
                    file: "png",
                  },
                ],
              },
            ],
          },
          {
            name: "Prod2",
            children: [
              {
                name: "Prod2_2",
                file: "png",
              },
            ],
          },
        ],
      },
      {
        name: "GroupBB",
        children: [
          {
            name: "Prod4",
          },
        ],
      },
    ],
  }),

  created() {
    console.log(MainGroup.items);
    console.log(SubGroup.items);
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
    console.log(treeData);
  },
};
</script>

<style scoped>
.v-treeview {
  max-width: 50%;
}
</style>