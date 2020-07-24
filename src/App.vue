<template>
  <div
          id="app"
          class="app"
  >
    <div class="container">
      <h1>Demo</h1>

      <p>Adding and removing tags:</p>

      <form-text-tags
              v-model="textBoxTags"
              :blur-on-add="true"
              :hide-text-on-blur="true"
              :clear-text-on-blur="false"
              :placeholder="'Add tags here'"
      >
        <template #label>
          Tags
        </template>
      </form-text-tags>

        <p>Textbox value:</p>

        <pre>{{ textBoxTags }}</pre>
    </div>
  </div>
</template>

<script>
  import FormTextTags from '@/components/FormTextTags';

  export default {
    name: 'App',
    components: {
      FormTextTags,
    },
    data() {
      return {
        textBoxTags: [
                "Apple",
                "Orange",
                "Banana",
        ],
      };
    },
  };
</script>

<style lang="less">
  @import '../src/less/app.less';
</style>
