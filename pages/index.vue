<template>
  <div>
    <NotionRenderer :blockMap="blockMap" fullPage />
    <h2 class="text-2xl text-center">Leave your comments!</h2>
    <div class="flex flex-col w-1/2 pl-4 mx-auto">
      <label for="username">Username:</label>
      <input v-model="username" class="border" name="username" type="text" />
      <label for="email">Email:</label>
      <input v-model="email" class="border" name="email" type="text" />
      <label for="email">Comment:</label>
      <textarea
        v-model="comment"
        class="border"
        name="comment"
        cols="3"
        rows="3"
        maxlength="1000"
      />
      <button @click="submitComment" class="rounded bg-blue text-md border p-3">
        Submit
      </button>
    </div>
    <div v-for="comment in comments" :key="comment.id">
      <p>{{ comment.username }}</p>
      <p>{{ comment.email }}</p>
      <p>{{ comment.comment }}</p>
    </div>
  </div>
</template>

<script>
const POST_ID = "13de327a4d534d4889d3dbc0e805afbd"

export default {
  data: () => ({
    blockMap: null,
    comments: [],
    username: "",
    comment: "",
    email: "",
  }),
  methods: {
    async submitComment() {
      try {
        await this.$supabase.from("comment").insert({
          post_id: POST_ID,
          username: this.username,
          email: this.email,
          comment: this.comment,
        });
      } catch (error) {
        console.error(error);
      }

      this.username = "";
      this.email = "";
      this.comment = "";
    },
    async getComments() {
      const comments = await this.$supabase.from("comment").insert({
        post_id: POST_ID,
        username: this.username,
        email: this.email,
        comment: this.comment,
      });

      this.comments = comments;
    },
  },
  async asyncData({ $notion }) {
    // use Notion module to get Notion blocks from the API via a Notion pageId
    const blockMap = await $notion.getPageBlocks(
      POST_ID,
    );
    return { blockMap };
  },
  async mounted() {
    const comments = await this.$supabase
      .from("comment")
      .select()
      .eq("post_id", POST_ID);

    this.comments = comments.body;
  },
};
</script>

<style>
@import "vue-notion/src/styles.css"; /* optional Notion-like styles */
</style>
