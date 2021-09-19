<template>
  <div
    class="file"
  >
    <div class="file__content">
      <div class="file__image">
        <img src="../svg/299060_folder_icon.svg" />
      </div>
      <p class="file__name">
        {{ name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FileView',
  props: {
    name: String
  }
}
</script>

