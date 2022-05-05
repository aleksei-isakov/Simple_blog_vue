<template>
  <div class="blog-entry-wrapper" @click="onClickOpenPost">
    <div class="blog-entry-content">
      <div class="title-container">
        <h1 class="blog-entry-title">{{ blogTitle }}</h1>
      </div>
      <hr />
      <p class="blog-entry-date">{{ date }}</p>

      <h3 class="blog-entry-short">{{ blogSubtitle }}</h3>

      <hr />
      <div class="comment-block">
        <img class="comments-icon" src="@/assets/bubble-comments.png" alt="ф" />
        <p>Comments: {{ comments.length }}</p>
      </div>
    </div>
    <div class="post-delete-icon-container">
      <button @click="onClickDeletePost" class="post-delete-icon">
        &times
      </button>
    </div>
    <modal-window class="full-post" :is-modal-visible="isModalVisible">
      <BlogFull
        @deleteComment="onClickDeleteComment"
        @commentData="commentData"
        @editComments="onClickEditPost"
        :title="title"
        :subtitle="subtitle"
        :body="body"
        :date="date"
        :id="id"
        :comments="comments"
      />
    </modal-window>
  </div>
</template>

<script>
import ModalWindow from "@/components/ModalWindow/ModalWindow";
import BlogFull from "@/components/BlogFull/BlogFull";
export default {
  name: "BlogEntry",
  components: { BlogFull, ModalWindow },
  props: {
    title: {
      type: [String, Number],
      default: "",
      required: true,
    },
    subtitle: {
      type: [String, Number],
      default: "",
      required: true,
    },
    body: {
      type: [String, Number],
      default: "",
      required: true,
    },
    date: {
      type: [Date, String, Number],
      default: "",
      required: true,
    },
    id: {
      type: Number,
      default: "",
      required: true,
    },
    comments: {
      type: Array,
      default: () => [],
    },
  },

  data: () => ({
    isModalVisible: false,
  }),

  methods: {
    onClickDeletePost() {
      this.$emit("deletePost", this.id);
    },
    onClickOpenPost() {
      this.isModalVisible = !this.isModalVisible;
    },
    commentData(commentData, id) {
      this.$emit("commentData", commentData, id);
    },
    onClickDeleteComment(commentId) {
      this.$emit('deleteComment', this.id, commentId)
    },
    onClickEditPost(postId, title, subtitle, body) {
      this.$emit('onClickEditPost', postId, title, subtitle, body)
    }
  },

  computed: {
    blogTitle() {
      return this.title.length < 20
        ? this.title
        : `${this.title.slice(0, 20)}...`;
    },
    blogSubtitle() {
      return this.subtitle.length < 142
        ? this.subtitle
        : `${this.subtitle.slice(0, 142)}...`;
    },
  },
};
</script>

<style scoped>
.blog-entry-wrapper {
  position: relative;
  display: flex;
  justify-content: center;
  min-height: 300px;
  max-height: 400px;
  width: 600px;
  border: 1px solid teal;
  border-radius: 10px;
  color: teal;
  background-image: url("@/assets/blog_backdrop.jpg");
  overflow: hidden;
}
.blog-entry-wrapper:hover {
  cursor: pointer;
}
.full-post:hover {
  cursor: initial;
}
.blog-entry-content {
  padding: 10px;
  display: flex;
  justify-content: space-evenly;
  flex-direction: column;
  gap: 30px;
  width: 480px;
  min-height: 298px;
  max-height: 398px;
  background-color: wheat;
}
.title-container {
  text-align: center;
  text-transform: capitalize;
}
.comment-block {
  display: flex;
  gap: 5px;
}
.comments-icon {
  width: 20px;
  height: 20px;
}
.post-delete-icon-container {
  position: absolute;
  right: 0;
  top: 0;
  width: 20px;
  height: 20px;
  border-radius: 10px;
}
.post-delete-icon {
  height: 100%;
  width: 100%;
  background-color: red;
}
.post-delete-icon:hover {
  cursor: pointer;
}
</style>
