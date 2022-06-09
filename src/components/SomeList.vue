<template>

  <transition-group tag="ul" name="users">
    <li
      v-for="user in users"
      :key="user"
      @click="removeUser(user)"
    >{{ user }}</li>
  </transition-group>

  <div>
    <input type="text" ref="inputUser" />
    <button @click="addUser">Add User</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      users: ['Stef', 'Ivan', 'Emil', 'Dani', 'Elena', 'Silvia', 'George']
    };
  },
  methods: {
    addUser () {
      this.users.unshift(this.$refs.inputUser.value);
    },
    removeUser (name) {
      this.users = this.users.filter(user => user !== name);
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 1rem 0;
  padding: 0;
}

li {
  border: 1px solid #ccc;
  padding: 1rem;
  text-align: center;
  width: 100px;
}

li:hover {
  background-color: lavender;
  cursor: pointer;
}

input {
  margin: 0 1rem;
}

.users-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.users-enter-active {
  transition: all 0.6s ease-out;
}

.users-enter-to {
  opacity: 1;
}

.users-leave-from {
  opacity: 1;
  width: 100px;
}

.users-move,
.users-leave-active {
  transition: all 0.6s ease-in;
}

.users-leave-active {
  position: absolute;
}

.users-leave-to {
  opacity: 0;
  transform: translateX(50px);
}

</style>