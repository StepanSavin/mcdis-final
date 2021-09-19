<template>
  <div
    @pointerup="selectFolder"
    @dblclick="selectFolder"
    @mousedown.prevent
    class="folder"
  >
    <div class="folder__content">
      <div class="folder__image">
        <img src="../svg/299060_folder_icon.svg" />
      </div>
      <p class="folder__name">
        {{ name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "FolderView",
  props: {
    name: String,
    index: Number,
  },

  methods: {
    selectFolder(event) {
      if (event.type === "pointerup" && screen.width > 1024) return;
      this.$emit("open", this.index);
    },

  },
};
</script>

