<template>
  <SideBar
    @update:isOpenTopic="openDialogTopic"
  />
  <MainContent
    :topics="this.topics"
    @update:isOpenComment="openDialogComment"
    @deleteTopic="requestToDeleteTopic"
  />
  <CommentDialog
    v-model:is-open="isOpenComment"
    @postComment="newComment=>this.requestToAddComment(newComment, this.currentId)"
  />
  <TopicDialog
    v-model:is-open="isOpenTopic"
    @postTopic="requestToAddTopic" />
</template>

<script>
import TopicDialog from '@/components/TopicDialog.vue'
import CommentDialog from '@/components/CommentDialog.vue'
import SideBar from '@/components/SideBar.vue'
import MainContent from '@/components/MainContent.vue'
export default {
  components: {
    MainContent,
    SideBar,
    CommentDialog,
    TopicDialog
  },
  data() {
    return {
      isOpenTopic: false,
      isOpenComment: false,
      topics: [],
      currentId: null,
      responseTime: 15,
      clientVersion: 0
    }
  },
  mounted() {
    this.requestLongPolling();
  },
  methods: {
    openDialogTopic() {
      this.isOpenTopic = true;
    },
    openDialogComment(currentId) {
      this.isOpenComment = true;
      this.currentId = currentId;
    },
    async request(options) {
      const response = await fetch(options.pathname, {
        method: options.method,
        headers: {
          'Content-Type': 'application/json' || 'text/plain'
        },
        body: options.body ? JSON.stringify(options.body) : null
      });
      if (!response.ok) {
        throw new Error('Request failed: ' + response.statusText);
      }
      if (options.method === 'GET') {
        return response.json();
      } else {
        return response.text();
      }
    },
    reportError(error) {
      if (error) {
        alert(error.toString());
      }
    },
    async requestLongPolling() {
      await this.requestToGetTopics();
      const longPolling = async () => {
        try {
          const currentTopics = await this.request({
            method: 'GET',
            pathname: `/longPolling?responseTime=${this.responseTime}&clientVersion=${this.clientVersion}`
          });
          if (currentTopics.currentVersion > this.clientVersion) {
            this.topics = [...currentTopics.topics];
            this.clientVersion = currentTopics.currentVersion;
          }
          await longPolling();
        } catch (err) {
          setTimeout(longPolling, 1000);
        }
      };
      await longPolling();
    },
    async requestToGetTopics() {
      try {
        const currentTopics = await this.request({
          method: 'GET',
          pathname: '/topics'
        });
        this.topics = [...currentTopics.topics];
        this.clientVersion = currentTopics.currentVersion;
      } catch (error) {
        this.reportError(error);
      }
    },
    async requestToAddTopic(newTopic) {
      console.log(newTopic)
      try {
        await this.request({
          method: 'POST',
          pathname: '/topics',
          body: newTopic
        });
      } catch (error) {
        this.reportError(error);
      }
    },
    async requestToAddComment(newComment, topicId) {
      try {
        await this.request({
          method: 'POST',
          pathname: `/topics/${topicId}/comment`,
          body: newComment
        });
      } catch (error) {
        this.reportError(error);
      }
    },
    async requestToDeleteTopic(topicId) {
      try {
        await this.request({
          method: 'DELETE',
          pathname: `/topics/${topicId}`
        });
      } catch (error) {
        this.reportError(error);
      }
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

.topics {
  display: flex;
  flex-direction: column;
  row-gap: 20px;
  width: 100%;
  overflow-y: auto;
}

.topic {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
  width: 100%;
  padding: 15px;
  border: 1px solid lightgray;
  border-radius: 10px;

  &__header {
    display: flex;
    flex-direction: column;
    row-gap: 5px;

    &-title {
      display: flex;
      align-items: center;
    }
  }

  &__body {
    &-commentary {
      display: flex;
      align-items: center;
      column-gap: 10px;
      font-style: italic;
    }
  }

  &__button {
    margin: auto;
  }
}
</style>