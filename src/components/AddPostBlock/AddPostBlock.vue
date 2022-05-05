<template>
  <div class="add-post-block-wrapper">
    <div class="add-post-block-container">
      <div class="add-post-block-text-area">
        <h2>Got something to say?</h2>
        <form action="submit">
          <textarea
            v-model="title"
            class="form_task"
            cols="60"
            rows="2"
            placeholder="Post title"
          >
          </textarea>

          <textarea
            v-model="subtitle"
            class="form_task"
            cols="60"
            rows="2"
            placeholder="About the post"
          ></textarea>
          <textarea
            v-model="body"
            class="form_task"
            cols="60"
            rows="10"
            placeholder="Write your post here"
          ></textarea>
        </form>
      </div>
      <div class="add-post-block-submit-button-area">
        <button class="submit-button" @click="OnClickSubmitModal">
          Submit post
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AddPostBlock",

  data: () => ({
    title: "",
    subtitle: "",
    body: "",
    id: Math.random(),
  }),
  methods: {
    OnClickSubmitModal() {
      const formData = {
        title: this.title,
        subtitle: this.subtitle,
        body: this.body,
        id: this.id,
        date: new Date(Date.now()).toLocaleString(),
        comments: [],
      }
      if (this.title && this.subtitle && this.body) {
        this.$emit("addPost", formData);
        this.$emit("onClickCloseModal");
      }
    },
  },
};
</script>

<style scoped>
.add-post-block-wrapper {
  min-height: 300px;
  width: 700px;
  padding: 10px 40px;
  border: 1px solid teal;
  border-radius: 10px;
  background-color: darkorange;
}
.add-post-block-container {
  display: flex;
}
.add-post-block-text-area {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
.add-post-block-submit-button-area {
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
  padding-bottom: 5px;
  width: 100%;
}
.submit-button {
  width: 120px;
  height: 50px;
  border: 1px solid teal;
  text-transform: uppercase;
}

</style>