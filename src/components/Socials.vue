<template>
    <div class="socialsWrapper">
      <a href="https://github.com/KrystianJonca" class="social">
        <font-awesome-icon :class="{ dark }" :icon="{ prefix: 'fab', iconName: 'github' }"/>
      </a>
    </div>
</template>

<script>
export default {
  name: 'Socials',
  props: {
    dark: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
.socialsWrapper {
  position: absolute;
  top: 0;
  right: 0;
  .social{
    color: #fff;
    font-size: 23px;
  }
}
.dark{
  color: #000;
}
</style>
