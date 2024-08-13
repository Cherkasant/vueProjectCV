<template>
  <div class="card card-w70">
    <template v-if="info.length">
      <component v-for="component in info" :is="'app-'+ component.type" v-bind="{value:component.text}"
                 :key="component.id">
      </component>
    </template>
    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
import AppTitle from "@/components/AppTitle";
import AppAvatar from "@/components/AppAvatar";
import AppSubtitle from "@/components/AppSubtitle";
import AppText from "@/components/AppText";

export default {
  name: "AppCard",
  props: ["info"],
  components: {AppTitle, AppAvatar, AppSubtitle, AppText},
};
</script>

<style scoped></style>
