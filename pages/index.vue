<template>
  <div>yes</div>
</template>

<script>
import NotionToHTML from "notion-page-to-html";

export default {
  created: async function () {
    const { title, icon, cover, html } = await NotionToHTML.convert(
      "https://albertoromero.notion.site/The-importance-of-designing-towards-abstractions-e4916a0b0e2d44eca2a9d17985c75c33"
    );

    console.log({ title, icon, cover, html });
  },
};
</script>
