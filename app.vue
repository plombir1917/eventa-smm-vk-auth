<template>
  <div class="vk-auth-wrapper">
    <div id="VkIdSdkOneTap"></div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import { useHead } from '@vueuse/head';

useHead({
  title: 'Авторизация ВКонтакте | Эвента - SMM бот для Telegram',
  meta: [
    {
      name: 'description',
      content:
        'Авторизация через ВКонтакте для SMM бота Эвенты в Telegram. Безопасный вход в систему.',
    },
    {
      name: 'keywords',
      content: 'авторизация вк, эвенты, smm бот, telegram, вход',
    },
  ],
});

onMounted(async () => {
  const VKID = await import('@vkid/sdk');

  VKID.Config.init({
    app: 53404751,
    redirectUrl: `https://reutov2f.beget.tech`,
    scope: 'wall groups photos',
  });

  const oneTap = new VKID.OneTap();
  const container = document.getElementById('VkIdSdkOneTap');

  if (container) {
    oneTap
      .render({
        container: container,
        scheme: VKID.Scheme.LIGHT,
        lang: VKID.Languages.RUS,
      })
      .on(VKID.WidgetEvents.ERROR, (err) => {
        console.error('VKID Error:', err);
      });
  }
});
</script>

<style>
.vk-auth-wrapper {
  max-width: 400px;
  width: 90%;
  margin: 0 auto;
  padding: 20px;
}

@media (max-width: 480px) {
  .vk-auth-wrapper {
    width: 95%;
    padding: 10px;
  }
}
</style>
