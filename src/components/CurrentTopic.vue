<template>
  <div class="current__topic">
    <div class="current__topic-header">
      <button class="current__topic-back" @click="hideTopic">
        Back
      </button>
    </div>
    <div class="current__topic-body">
      <div class="current__topic-avatar">
        <img src="https://bootdey.com/img/Content/avatar/avatar5.png" alt="Avatar" class="current__topic-img" width="100">
      </div>
      <div class="current__topic-content">
        <div class="current__topic-info">
          <h2 class="current__topic-title">{{ currentTopic.title }}</h2>
          <button class="current__topic-delete-topic" @click="deleteTopic(currentTopic.id)">Delete Topic</button>
        </div>
        <p class="current__topic-summary">{{ currentTopic.summary }}</p>
        <p class="current__topic-author">{{ currentTopic.author }}</p>
        <button class="current__topic-add-comment" @click="openDialog(currentTopic.id)">Add Comment</button>
      </div>
    </div>
    <div class="current__comments">
      <div class="comment" v-for="comment in currentTopic.comments" :key="comment.id">
        <div class="comment__avatar">
          <img src="https://bootdey.com/img/Content/avatar/avatar3.png" alt="Avatar" class="comment__avatar-img" width="100">
        </div>
        <div class="comment__content">
          <p class="comment__author"><strong>{{ comment.author }}</strong></p>
          <p class="comment__message">{{ comment.message }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    currentTopic: {
      type: Object,
      required: true
    }
  },
  methods: {
    hideTopic() {
      this.$emit('hideTopic')
    },
    openDialog(id) {
      this.$emit('openDialog', id);
    },
    deleteTopic(id) {
      this.$emit('deleteTopic', id);
      this.hideTopic();
    }
  }
}
</script>

<style lang="scss" scoped>
button {
  display: inline-block;
  width: 200px;
  height: 45px;
  margin: 10px 20px;
  text-decoration: none;
  line-height: 45px;
  letter-spacing: 3px;
  border: 1px solid gray;
  border-radius: 45px;
  font-weight: 600;
  font-size: 11px;
  color: gray;
  background: inherit;
  text-transform: uppercase;
  text-align: center;
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: 0.3s;

  &:hover {
    background: #007bff;
    box-shadow: 0 5px 20px rgba(135, 206, 250, 1);
    color: white;
    transform: translateY(-5px);
  }
}

.current__topic {
  display: flex;
  flex-direction: column;
  row-gap: 15px;


  &-body {
    display: flex;
    column-gap: 20px;
    padding: 15px;
    background-color: #ffffff;
    border-radius: 10px;
    box-shadow:
      0 1px 3px 0 rgba(0, 0, 0, 0.1),
      0 1px 2px 0 rgba(0, 0, 0, 0.06);
  }

  &-img {
    border-radius: 50%;
  }

  &-content {
    display: flex;
    flex-direction: column;
    row-gap: 10px;
  }

  &-info {
    display: flex;
    align-items: center;
  }

  &-summary, &-author {
    color: gray;
  }

  &-add-comment, &-back {
    margin-inline: 0;
  }

  &-back {
    background-color: #ffffff;
  }
}

.current__comments {
  display: flex;
  flex-direction: column;
  row-gap: 15px;
}

.comment {
  display: flex;
  column-gap: 20px;
  padding: 15px;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow:
    0 1px 3px 0 rgba(0, 0, 0, 0.1),
    0 1px 2px 0 rgba(0, 0, 0, 0.06);

  &__avatar-img {
    border-radius: 50%;
  }

  &__content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    row-gap: 10px;
  }
}
</style>