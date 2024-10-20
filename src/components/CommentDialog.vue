<template>
  <UniversalDialog
    :isOpen="isOpen"
    :form="form"
    :initData="newComment"
    @postData="postComment"
    @update:isOpen="updateDialog"
    />
</template>

<script>
import UniversalDialog from '@/components/UniversalDialog.vue'

export default {
  components: { UniversalDialog },
  props: {
    isOpen: {
      type: Boolean,
      default: false,
      required: true
    }
  },
  data() {
    return {
      newComment: {
        author: '',
        message: ''
      },
      form: {
        title: 'New Commentary',
        items: [
          { label: 'Your name', id: 'author', model: 'author', placeholder: 'Enter Your Name' },
          { label: 'Message', id: 'message', model: 'message', placeholder: 'Enter Message' }
        ]
      }
    }
  },
  methods: {
    updateDialog() {
      this.$emit('update:isOpen', false);
    },
    postComment(dataComment) {
      this.$emit('postComment', dataComment);
    }
  }
}
</script>

<style lang="scss" scoped>
</style>