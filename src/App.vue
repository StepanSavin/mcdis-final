<template >
  <div @click="selectActiveFile" class="main">
    <div class="container">
      <h1 class="folder-title">
        {{ selectedFolder.name ? selectedFolder.name : "Root Folder" }}
      </h1>
      <div class="navigation-panel">
        <button
          @click="backToFolder"
          class="navigation-panel__button"
          id="backButton"
        >
          Назад
        </button>
        <div class="navigation-panel__search">
          <input
            v-model="searchValue"
            placeholder="Поиск по папке"
            class="navigation-panel__input"
          />
          <img
            class="navigation-panel__icon"
            src="../public/svg/search-icon.svg"
          />
        </div>
      </div>
      <div class="file-container">
        <folder-view
          @click="selectActiveFile"
          @open="selectFolder"
          v-for="(folder, index) in filteredFolders"
          :key="folder"
          :index="index"
          :name="folder.name"
          :amountFolders="folder.folders.length"
          :amountFiles="folder.files.length"
        >
        </folder-view>
        <file-view
          @click="selectActiveFile"
          v-for="file in filteredFiles"
          :key="file"
          :name="file.name"
          :type="file.type"
          :length="file.length"
        >
        </file-view>
      </div>
      <h2
        v-if="filteredFiles.length === 0 && filteredFolders.length === 0"
        class="nothing"
      >
        Ничего не найдено
      </h2>
      <div v-if="isActiveFileSelected" class="note">
        Кликните два раза, чтобы открыть
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

      searchValue: "",

      isActiveFileSelected: false,
    };
  },

  computed: {
    filteredFolders() {
      return this.selectedFolder.folders.filter((item) => {
        return item.name.toUpperCase().includes(this.searchValue.toUpperCase());
      });
    },
    filteredFiles() {
      return this.selectedFolder.files.filter((item) => {
        return item.name.toUpperCase().includes(this.searchValue.toUpperCase());
      });
    },
  },

  methods: {
    selectFolder(index) {
      this.previousFolder.push(this.selectedFolder);
      this.selectedFolder = this.selectedFolder.folders[index];
      this.defineButtonEnable();
      this.searchValue = "";
      this.isActiveFileSelected = false;
    },

    backToFolder() {
      if (this.previousFolder.length === 0) return;
      this.selectedFolder = this.previousFolder[this.previousFolder.length - 1];
      this.previousFolder.pop();
      this.defineButtonEnable();
      this.searchValue = "";
    },

    selectActiveFile(event) {
      event.stopPropagation();
      if (window.innerWidth <= 1024) return;

      this.resetActiveClasses();
      if (event.currentTarget.classList.contains("main")) {
        this.isActiveFileSelected = false;
        return;
      }
      event.currentTarget.classList.add("active");
      if (event.currentTarget.classList.contains("folder")) {
        this.isActiveFileSelected = true;
      } else {
        this.isActiveFileSelected = false;
      }
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

    defineButtonEnable() {
      if (this.previousFolder.length === 0) {
        document.querySelector("#backButton").disabled = true;
      } else {
        document.querySelector("#backButton").disabled = false;
      }
    },
  },

  mounted() {
    this.defineButtonEnable();
  },
};
</script>

<style lang="scss">
@import url("./styles/main.scss");
</style>
