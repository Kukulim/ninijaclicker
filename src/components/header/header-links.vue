<template>
<div>
  <a
    :href="github"
    target="_blank"
    class="nav-link"
  >
    Github
  </a>
</div>
</template>

<script>
export default {
  data() {
    return {
      github: 'https://github.com/Kukulim',
    };
  },
};
</script>

