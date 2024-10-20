<template>
  <UniversalDialog
    :isOpen="isOpen"
    :form="form"
    :initData="newTopic"
    @postData="postTopic"
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
      required: true,
      default: false
    }
  },
  data() {
    return {
      newTopic: {
        author: '',
        title: '',
        summary: ''
      },
      form: {
        title: 'New Discussion',
        items: [
          { label: 'Your name', id: 'author', model: 'author', placeholder: 'Enter Your Name' },
          { label: 'Title', id: 'title', model: 'title', placeholder: 'Enter Title' },
          { label: 'Summary', id: 'summary', model: 'summary', placeholder: 'Enter Summary' }
        ]
      }
    }
  },
  methods: {
    updateDialog() {
      this.$emit('update:isOpen', false);
    },
    postTopic(dataTopic) {
      this.$emit('postTopic', dataTopic);
    }
  }
}
</script>

<style lang="scss" scoped>
</style>