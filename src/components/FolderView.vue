<template>
  <div class="folder">
    {{ name }}
  </div>
</template>

<script>
export default {
  name: "FolderView",
  props: {
    name: String,
    folders: Array,
  },

  methods: {
    selectFolder() {
      this.$emit("open");
      console.log("select");
    },
  },
};
</script>

