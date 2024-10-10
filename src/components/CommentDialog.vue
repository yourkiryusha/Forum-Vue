<template>
  <dialog
    @click="hideDialog"
    v-if="isShow"
    class="dialog"
  >
    <form class="dialog__form" @click.stop @submit.prevent="postComment">
      <div class="form__header">
        <p class="form__header-title">New Commentary</p>
        <button
          @click="hideDialog"
          class="form__header-button"
          type="button">
        </button>
      </div>
      <div class="form__body">
        <ul class="form__list">
          <li class="form__item">
            <label for="name" class="form__item-label">Your name</label>
            <input
              v-model="comment.author"
              class="form__item-input"
              type="text"
              id="name"
              placeholder="Enter Your Name"
              required
            />
          </li>
          <li class="form__item">
            <label for="title" class="form__item-label">Message</label>
            <input
              v-model="comment.message"
              class="form__item-input"
              type="text"
              id="title"
              placeholder="Enter Message"
              required
            />
          </li>
        </ul>
      </div>
      <div class="form__footer">
        <button
          @click="hideDialog"
          class="form__button-cancel"
          type="button"
        >
          Cancel
        </button>
        <button
          class="form__button-post"
          type="submit"
        >
          Post
        </button>
      </div>
    </form>
  </dialog>
</template>

<script>
export default {
  props: {
    isShow: {
      type: Boolean,
      default: false,
      required: true
    }
  },
  data() {
    return {
      comment: {
        author: '',
        message: ''
      }
    }
  },
  methods: {
    hideDialog() {
      this.$emit('update:isShow', false);
    },
    postComment() {
      this.$emit('postComment', this.comment);
      this.comment = {
        author: '',
        message: ''
      };
      this.hideDialog();
    }
  }
}
</script>

<style lang="scss" scoped>
.dialog {
  position: fixed;
  display: flex;
  width: 100vw;
  height: 100vh;
  border: none;
  background: rgba(0, 0, 0, 0.5);

  &__form {
    min-width: 800px;
    display: flex;
    flex-direction: column;
    row-gap: 15px;
    margin: auto;
    background: #fff;
  }
}

.form {
  &__header {
    display: flex;
    justify-content: space-between;
    padding: 20px;
    font-size: 18px;
    color: #fff;
    background-color: #007bff;

    &-button {
      position: relative;
      min-width: 25px;
      min-height: 25px;
      background-color: inherit;
      border: none;
      cursor: pointer;

      &::after {
        position: absolute;
        content: "";
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: url('./icons/cross-button.svg') center/contain no-repeat;
      }
    }
  }

  &__list {
    display: flex;
    flex-direction: column;
    row-gap: 15px;
    padding: 10px 20px;
  }

  &__item {
    display: flex;
    flex-direction: column;
    row-gap: 10px;

    &-input {
      min-height: 35px;
      padding: 0 10px;
      border: 1px solid lightgray;
      border-radius: 5px;
      font-size: 14px;

      &:hover,
      &:focus {
        outline: none;
      }
    }
  }

  &__footer {
    display: flex;
    align-self: flex-end;
    column-gap: 10px;
    padding: 10px 20px;
  }

  &__button-cancel {
    min-width: 75px;
    min-height: 30px;
    border-radius: 5px;
    font-size: 14px;
    cursor: pointer;
    transition-duration: 0.3s;
    background-color: #ffffff;
    color: inherit;
    border: none;

    &:hover {
      background-color: #e2e8f0;
    }
  }

  &__button-post {
    min-width: 75px;
    min-height: 30px;
    border: 1px solid #007bff;
    border-radius: 5px;
    background-color: #007bff;
    color: #ffffff;
    font-size: 14px;
    cursor: pointer;
    transition-duration: 0.3s;

    &:hover {
      color: #e2e8f0;
    }
  }
}
</style>