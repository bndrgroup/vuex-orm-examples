<template>
  <div class="UsersList">
    <div class="user" :key="user.id" v-for="user in users">
      <input
        class="input"
        :value="user.name"
        placeholder="Type in user's name!"
        @input="e => { update(user, e.target.value) }"
      >

      <p class="tasks">{{ user.todos.length }} Tasks</p>

      <button class="icon" @click="destroy(user)">
        <IconTrash class="trash" />
      </button>
    </div>
  </div>
</template>

<script>
import User from '@/models/User'
import IconTrash from './icons/IconTrash'

export default {
  components: {
    IconTrash
  },

  computed: {
    users () {
      return User.query().with('todos').orderBy('id', 'desc').get()
    }
  },

  methods: {
    update (user, name) {
      user.$update({ name })
    },

    destroy (user) {
      user.$delete()
    }
  }
}
</script>

<style scoped>
@import "styles/variables";

.user {
  display: flex;
  align-items: center;
  border-top: 1px solid var(--c-divider);
}

.user:hover {
  background-color: #fafafa;
}

.user:hover .trash {
  opacity: 1;
}

.input {
  flex-grow: 1;
  border: 0;
  padding: 12px 24px;
  width: 100%;
  background-color: transparent;
  transition: all .3s;
}

.tasks {
  font-size: 12px;
  white-space: nowrap;
  color: var(--c-gray);
}

.icon {
  display: block;
  padding: 12px 24px;
}

.icon:hover .trash {
  fill: var(--c-black);
}

.trash {
  width: 14px;
  height: 14px;
  opacity: 0;
  transform: translateY(2px);
  transition: all .3s;
  fill: var(--c-gray);
}
</style>
