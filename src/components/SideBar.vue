<template>
  <div class="sidebar">
    <div class="sidebar__header">
      <button class="sidebar__button" type="button" @click="openDialog">New Discussion</button>
    </div>
    <div class="sidebar__body">
      <nav class="sidebar__nav">
        <a href="#" class="sidebar__nav-link active">All Threads</a>
      </nav>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    openDialog() {
      this.$emit('update:isOpenTopic');
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

.sidebar {
  display: flex;
  flex-direction: column;

  &__header {
    border: 1px solid lightgray;
    border-bottom: none;
  }

  &__body {
    height: 100%;
    padding: 20px;
    border: 1px solid lightgray;
  }

  &__nav {
    display: flex;
    flex-direction: column;

    &-link {
      padding: 8px 5px;
      color: gray;
      border-radius: 5px;
      text-decoration: none;
      transition-duration: 0.3s;
    }
  }
}

.active {
  color: #0056b3;
  background-color: #e2e8f0;
}
</style>