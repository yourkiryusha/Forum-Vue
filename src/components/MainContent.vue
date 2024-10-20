<template>
  <div class="main">
    <div class="main__header">
      <select name="select" class="main__header-select" v-model="sortByOption">
        <option value="" disabled>Select from the list</option>
        <option
          v-for="(option, index) in options"
          :key="index"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>
      <input type="text" class="main__header-input" placeholder="Search forum" v-model.trim="searchByTitle"/>
    </div>
    <div class="main__body">
      <div class="topics" v-if="!currentTopic">
        <div
          class="topic"
          v-for="topic in filteredAndSortedTopics"
          :key="topic.id"
          :id="topic.id"
        >
          <a href="#" class="topic__link" @click.prevent="openTopic(topic)">
            <img src="https://bootdey.com/img/Content/avatar/avatar5.png" alt="Avatar" class="topic__img" width="100" />
          </a>
          <div class="topic__content">
            <a href="#" class="topic__title" @click.prevent="openTopic(topic)">
              <h1 class="topic__title-name">{{ topic.title }}</h1>
            </a>
            <p class="topic__description">{{ topic.summary }}</p>
            <p class="topic__author"><strong>created by </strong>{{ topic.author }}</p>
          </div>
          <p class="topic__comments">{{ topic.comments.length }}</p>
        </div>
      </div>
      <CurrentTopic
        v-if="currentTopic"
        :currentTopic="currentTopic"
        :key="currentTopic.id"
        :id="currentTopic.id"
        @hideTopic="hideTopic"
        @openDialog="openDialog"
        @deleteTopic="deleteTopic"/>
    </div>
  </div>
</template>

<script>
import CurrentTopic from '@/components/CurrentTopic.vue'

export default {
  components: { CurrentTopic },
  props: {
    topics: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      currentTopic: null,
      searchByTitle: '',
      sortByOption: '',
      options: [
        { value: 'noReplies', label: 'No replies yet' },
        { value: 'mostDiscussed', label: 'Most discussed' }
      ]
    }
  },
  watch: {
    topics: {
      handler(updatedTopics) {
        if (this.currentTopic) {
          const updatedTopic = updatedTopics.find(topic => topic.id === this.currentTopic.id);
          if (updatedTopic) {
            this.currentTopic = updatedTopic;
          } else {
            this.currentTopic = null;
          }
        }
      },
      deep: true
    }
  },
  computed: {
    filteredAndSortedTopics() {
      let sortedTopics = [...this.topics];

      if (this.searchByTitle) {
        sortedTopics =  this.topics.filter(currentTopic => {
          return currentTopic.title.toLocaleLowerCase().includes(this.searchByTitle.toLowerCase());
        });
      }

      switch (this.sortByOption) {
        case '':
          return sortedTopics;
        case 'noReplies':
          return sortedTopics.filter(currentTopic => currentTopic.comments.length === 0);
        case 'mostDiscussed':
          return sortedTopics.sort((firstTopic, secondTopic) => secondTopic.comments.length - firstTopic.comments.length);
        default:
          return sortedTopics;
      }
    }
  },
  methods: {
    openTopic(topic) {
      this.currentTopic = topic;
    },
    hideTopic() {
      this.currentTopic = null;
    },
    openDialog(id) {
      this.$emit('update:isOpenComment', id);
    },
    deleteTopic(id) {
      this.$emit('deleteTopic', id);
    }
  }
}
</script>

<style lang="scss" scoped>
.main {
  display: flex;
  flex-direction: column;
  width: 100%;

  &__header {
    display: flex;
    justify-content: space-between;
    padding: 10px 20px;
    border: 1px solid lightgray;
    border-left: none;

    &-select,
    &-input {
      width: 200px;
      height: 45px;
      color: gray;
      border: 1px solid gray;
      border-radius: 15px;
    }

    &-input {
      width: 250px;
      padding: 10px;

      &:hover,
      &:focus {
        outline: none;
      }
    }
  }

  &__body {
    height: 100vh;
    padding: 15px;
    border-right: 1px solid lightgray;
    background-color: #e2e8f0;
    overflow-y: auto;
  }
}

.topics {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
}

.topic {
  display: flex;
  justify-content: start;
  column-gap: 20px;
  padding: 15px;
  border: 1px solid lightgray;
  border-radius: 10px;
  background-color: white;
  box-shadow:
    0 1px 3px 0 rgba(0, 0, 0, 0.1),
    0 1px 2px 0 rgba(0, 0, 0, 0.06);

  &__link {
    display: flex;
    align-items: center;
  }

  &__img {
    border-radius: 50%;
  }

  &__content {
    display: flex;
    flex-direction: column;
    row-gap: 5px;
  }

  &__title {
    text-decoration: none;
    color: currentColor;
  }

  &__description {
    color: gray;
  }

  &__comments {
    display: flex;
    align-items: center;
    position: relative;
    margin-left: auto;

    &::before {
      position: absolute;
      content: '';
      background: url('./icons/message.svg') center/contain no-repeat;
      top: 50%;
      right: 100%;
      width: 25px;
      height: 25px;
      transform: translate(-25%, -50%);
    }
  }
}
</style>