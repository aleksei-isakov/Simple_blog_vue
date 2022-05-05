<template>
  <div class="home_page_wrapper">
    <button
        id="add-post-button"
        @click="onClickToggleModal"
    >
      Add post
    </button>

    <modal-window
        :is-modal-visible="isModalVisible"
        @click="onClickToggleModal"
    >
      <add-post-block
          @onClickCloseModal="onClickToggleModal"
          @addPost="addPost"
      />

    </modal-window>

    <BlogEntry
        @deletePost="onClickDeletePost"
        @commentData="addComment"
        @deleteComment="deleteComment"
        @onClickEditPost="editPost"
        class="blog-entry"
        :id="post.id"
        :date="post.date"
        :title="post.title"
        :body="post.body"
        :subtitle="post.subtitle"
        :comments="post.comments"
        :comment-body="post.commentBody"
        :comment-name="post.commentName"
        v-for="post of postList"
        :key="post.id"
    />

  </div>
</template>

<script>

import BlogEntry from "@/components/BlogEntry/BlogEntry";
import AddPostBlock from "@/components/AddPostBlock/AddPostBlock";
import ModalWindow from "@/components/ModalWindow/ModalWindow";

export default {

  name: "HomePage",

  components: {ModalWindow, AddPostBlock, BlogEntry},

  data: () => ({
    isModalVisible: false,
    postList: [],
  }),

  methods: {
    onClickToggleModal() {
    this.isModalVisible = !this.isModalVisible
    },
    addPost(newPost) {
      this.postList.push(newPost)
    },
    onClickDeletePost(item) {
      this.postList = this.postList.filter(element => element.id !== item)
    },
    addComment(comment, postId) {
        const post = this.postList.find(post =>
          post.id === postId)
      if (post) {
        post.comments.push(comment)
      }
    },
    deleteComment(postId, commentId) {
      const post = this.postList.find(post =>
          post.id === postId)
      if (post) {
        post.comments = post.comments.filter(element => element.commentId !== commentId)
        }
      },
    editPost(postId, title, subtitle, body) {
      const post = this.postList.find(post => post.id === postId)
      post.title = title
      post.subtitle = subtitle
      post.body = body
    }
  },

  watch: {
    postList: {
      handler() {
        localStorage.setItem('postList', JSON.stringify(this.postList));
      },
      deep: true
    }
  },

  mounted() {
    if (localStorage.postList) {
      this.postList = JSON.parse(localStorage.postList)
    }
  },

}
</script>

<style scoped>
.home_page_wrapper{
  display: flex;
  flex-direction: column;
  position: relative;
  gap: 20px;
  padding: 20px 0 20px;
  min-height: 91vh;
}

#add-post-button {
  position: absolute;
  width: 80px;
  height: 80px;
  right: 50px;
  top: 50px;
  background-color: darkorange;
  color: teal;
  text-transform: uppercase;
  font-weight: bold;
  border-radius: 50%;
}
#add-post-button:hover {
 cursor:pointer
}
</style>