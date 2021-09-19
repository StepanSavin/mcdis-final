<template >
  <div class="container">
    <p @click="backToFolder" class="folder-title">
      {{ selectedFolder.name ? selectedFolder.name : "Root Folder" }}
    </p>
    <folder-view
      @click="selectFolder"
      v-for="(folder, index) in selectedFolder.folders"
      :key="folder"
      :data-number="index"
      :name="folder.name"
    >
    </folder-view>
    <file-view
      v-for="file in selectedFolder.files"
      :key="file"
      :name="file.name"
    >
    </file-view>
  </div>
</template>

<script>
import FolderView from "./components/FolderView.vue";
import FileView from "./components/FileView.vue";
import data from "./data/list";

export default {
  name: "App",

  components: {
    FolderView,
    FileView,
  },
  data() {
    return {
      data: data,
      selectedFolder: data,
      previousFolder: [],
    };
  },

  methods: {
    selectFolder(event) {
      const index = event.target.getAttribute("data-number");
      this.previousFolder.push(this.selectedFolder);
      this.selectedFolder = this.selectedFolder.folders[index];

      console.log(this.selectedFolder);
    },

    backToFolder() {
      if (this.previousFolder.length === 0) return;
      this.selectedFolder = this.previousFolder[this.previousFolder.length - 1];
      this.previousFolder.pop();
    },
  },
};
</script>

<style lang="scss">
@import url('./styles/main.scss');
</style>
