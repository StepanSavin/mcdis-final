<template >
  <div @click="selectActiveFile" class="main">
    <div class="container">
      <h1 @click="backToFolder" class="folder-title">
        {{ selectedFolder.name ? selectedFolder.name : "Root Folder" }}
      </h1>
      <div class="file-container">
        <folder-view
          @click="selectActiveFile"
          @open="selectFolder"
          v-for="(folder, index) in selectedFolder.folders"
          :key="folder"
          :index="index"
          :name="folder.name"
        >
        </folder-view>
        <file-view
          @click="selectActiveFile"
          v-for="file in selectedFolder.files"
          :key="file"
          :name="file.name"
        >
        </file-view>
      </div>
    </div>
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
    selectFolder(index) {
      this.previousFolder.push(this.selectedFolder);
      this.selectedFolder = this.selectedFolder.folders[index];
    },

    backToFolder() {
      if (this.previousFolder.length === 0) return;
      this.selectedFolder = this.previousFolder[this.previousFolder.length - 1];
      this.previousFolder.pop();
    },

    selectActiveFile(event) {
      event.stopPropagation();
      this.resetActiveClasses();
      console.log(event.currentTarget.classList.contains('main'));
      if (event.currentTarget.classList.contains('main')) return;
      event.currentTarget.classList.add("active");
    },

    resetActiveClasses() {
      let folders = document.querySelectorAll(".folder");
      let files = document.querySelectorAll(".file");
      for (let file of files) {
        file.classList.remove("active");
      }
      for (let folder of folders) {
        folder.classList.remove("active");
      }
    },
  },
};
</script>

<style lang="scss">
@import url("./styles/main.scss");
</style>
