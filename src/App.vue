<template>
  <div id="app" class="pt-3 pb-3">
    <input-form :ttl="title" />
  </div>
</template>

<script>
// inputFormをComponentとして設定
import InputForm from "@/components/InputForm.vue";

export default {
  name: "app",
  components: {
    InputForm
  },
  data() {
    return {
      // titleをpropとしてComponentに渡す
      title: "勤怠連絡 BOT"
    };
  }
};
</script>

<style>
#app {
  color: #2c3e50;
}
</style>