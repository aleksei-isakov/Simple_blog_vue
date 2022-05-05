<template>
  <div class="full-blog-wrapper">
    <div v-if="!isEditMode" class="blog-entry-content">
      <div class="title-container">
        <p class="blog-entry-date"> {{date}} </p>
        <h1 class="blog-entry-title">{{ title }}</h1>
      </div>
      <h3 class="blog-entry-short">{{ subtitle }}</h3>
      <button @click="onClickToggleEditMode" class="edit-button-container">
        <img class="icon" src="@/assets/edit-icon.png" title="Edit comment" alt="icon">
      </button>
      <hr>
      <p class="blog-entry-date"> {{body}} </p>
      <div class="comments">

        <hr>

        <div v-if="isCommentsOpen" class="comments-block">

          <div v-if="!comments.length">
            <h4> This post hasn't been commented on yet</h4>
          </div>

          <div v-else class="comment-list">
            <div v-for="comment in comments" class="comment-item">
              <div class="comment-body">
              <h4 class="comment-item-title"> @{{comment.commentatorName}}:</h4>
              <p> {{comment.commentatorBody}} </p>
              </div>
                <button class="comment-remove-button" :value="comment.commentId" @click="onClickDeleteComment">&times</button>
            </div>
          </div>
          <button @click="onClickOpenCommentEditor" class="comment-open-button"> Leave a comment </button>
        </div>


        <button @click="onClickShowComments" v-if="!isCommentsOpen" class="comment-button"> Show comments </button>
        <button  @click="onClickHideComments" v-else-if="isCommentsOpen" class="comment-button"> Collapse comments </button>

        <div v-if="isCommentEditorOpen && isCommentsOpen" class="new-comment-section">
          <hr>
          <h4>Leave a comment</h4>
          <form class="form" action="submit">
               <textarea
                   v-model="this.commentName"
                   class="comment-form-title"
                   cols="60"
                   rows="2"
                   placeholder="User name"
               >
                 </textarea>
            <textarea
                v-model="this.commentBody"
                class="comment-form-body"
                cols="60"
                rows="8"
                placeholder="Comment"
            >
                  </textarea>
          </form>
          <div class="post-cancel-container">
            <button @click="onClickPostComment" class="comment-post-button">Post</button>
            <button @click="onClickHideCommentEditor" class="comment-post-button">Cancel</button>
          </div>
        </div>

      </div>
    </div>



    <div v-if="isEditMode" class="blog-entry-content">
      <div class="title-container">
        <p class="blog-entry-date"> {{date}} </p>
        <h1 class="blog-entry-title">
          <textarea :value="title" id="Title" cols="88" rows="2"> </textarea>
          </h1>
      </div>
      <h3 class="blog-entry-short">
        <textarea maxlength="25" :value="subtitle" id="Subtitle" cols="88" rows="6"></textarea>
      </h3>
      <button @click="onClickToggleEditMode" class="edit-button-container">
        <img class="icon" src="@/assets/edit-icon.png" title="Edit comment" alt="icon">
      </button>
      <hr>
      <div class="blog-entry-date">
        <textarea id="Body" :value="body" cols="88" rows="10"  > </textarea>
      </div>
      <div class="comments">

        <hr>
        <button @click="onClickEditPost" class="comment-button"> Submit changes </button>
        <button @click="onClickToggleEditMode" class="cancel-edit-button"> Cancel </button>
      </div>
    </div>


  </div>

</template>

<script>
export default {
  name: "BlogFull",

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
    }

  },

  data: () => ({
    isPostOpen: true,
    isCommentsOpen: false,
    isCommentEditorOpen: false,
    isComments: false,
    commentName: '',
    commentBody: '',
    isEditMode: false,
    editedTitle: '',
    editedSubtitle: '',
    editedBody: '',
  }),

  methods: {
    onClickShowComments() {
      this.isCommentsOpen = true
    },
    onClickHideComments() {
      this.isCommentsOpen = false
    },
    onClickOpenCommentEditor() {
      this.isCommentEditorOpen = true
    },
    onClickHideCommentEditor() {
      this.isCommentEditorOpen = false
    },
    onClickPostComment() {
      const commentData = {
        commentatorName: this.commentName,
        commentatorBody: this.commentBody,
        commentId: new Date().toString(),
      }
      if (this.commentName &&  this.commentBody) {
        this.$emit('commentData', commentData, this.id)
        this.commentName = ''
        this.commentBody = ''
        this.isCommentEditorOpen = false
      }
    },
    onClickDeleteComment(e) {
      this.$emit('deleteComment', e.target.value)
    },
    onClickToggleEditMode() {
      this.isEditMode = !this.isEditMode
    },
    onClickEditPost() {
      let title = document.getElementById('Title').value;
      let subtitle = document.getElementById('Subtitle').value;
      let body = document.getElementById('Body').value;
      this.editedTitle = title
      this.editedSubtitle = subtitle
      this.editedBody = body
      this.$emit('editComments', this.id, this.editedTitle, this.editedSubtitle, this.editedBody)
      this.onClickToggleEditMode()
    }
  },

}
</script>

<style scoped>
.full-blog-wrapper {
  display: flex;
  position: relative;
  justify-content: space-between;
  min-height: 300px;
  width: 730px;
  padding: 10px;
  border: 1px solid teal;
  border-radius: 10px;
  background-color: teal;
  color: peachpuff;
}
.blog-entry-content {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 30px;
  width: 100%;
}
.blog-entry-date{
  margin-bottom: 20px;
}
.comments {
  min-height: 60px;
}
.comment-button {
  position: absolute;
  bottom: 20px;
  height: 30px;
}
.comments-block {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 50px;
  margin: 50px 0 ;
}
.comment-open-button{
  height: 30px;
  width: 130px;
  margin: 20px 0;
}
.new-comment-section {
  display: flex;
  flex-direction: column;
  gap: 20px;
  min-height: 300px;
  margin-bottom: 40px;
}
.comment-post-button{
  width: 100px;
}
.form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.post-cancel-container {
  display: flex;
  justify-content: space-between;
}
.comment-list {
  display: flex;
  flex-direction: column;
  margin: 0 0 30px 0;
  overflow: hidden;
  gap: 25px
}
.comment-item {
  display: flex;
  justify-content: space-between;
  width: 100%;
  border: 1px solid wheat;
  border-radius: 10px;
  min-height: 50px;
  padding: 5px;
  gap: 20px
}
.comment-remove-button{
  font-weight: bold;
  font-size: 20px;
  background: none;
  color: wheat;
}
.comment-remove-button:hover {
  cursor: pointer;
}
.comment-item-title {
  margin-bottom: 10px;
}
.edit-button-container{
  position: absolute;
  padding: 5px;
  right: 20px;
  top:20px;
  width: 50px;
  height: 40px;
  background: none;
}
.icon {
  width: 100%;
  height: 100%;
}
.icon:hover {
  cursor: pointer;
}
.cancel-edit-button {
  position: absolute;
  bottom: 20px;
  right: 30px;
  width: 100px;
  height: 30px;
}
.blog-entry-title {
  overflow: hidden;
}
</style>