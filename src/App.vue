<template>
  <div id="app" class="container mt-5">
    <button @click="openModal" class="btn btn-primary mb-4">Добавить пользователя</button>

    <UserModal :isOpen="isModalOpen" @close="closeModal">
      <div class="add-user-form">
        <h2 class="mb-4">Добавить нового пользователя</h2>
        <form @submit.prevent="addUser">
          <div class="mb-3">
            <input v-model="newUser.name" class="form-control" placeholder="Имя" required />
          </div>
          <div class="mb-3">
            <input v-model="newUser.age" class="form-control" placeholder="Возраст" type="number" required />
          </div>
          <div class="mb-3">
            <input v-model="newUser.city" class="form-control" placeholder="Город" required />
          </div>
          <div class="mb-3">
            <input v-model="newUser.email" class="form-control" placeholder="Email" type="email" required />
          </div>
          <button type="submit" class="btn btn-success">Добавить</button>
        </form>
      </div>
    </UserModal>

    <div class="mb-4">
      <input v-model="searchQuery" class="form-control" placeholder="Поиск по имени" />
    </div>

    <div class="row">
      <div class="col-md-4 mb-4" v-for="(user, index) in filteredUsers" :key="index">
        <UserCard>
          <template v-slot:header>
            <h2>{{ user.name }}</h2>
          </template>

          <template v-slot:default>
            <p>Возраст: {{ user.age }} лет</p>
            <p>Город: {{ user.city }}</p>
          </template>

          <template v-slot:footer>
            <p>Контакты: {{ user.email }}</p>
          </template>
        </UserCard>
      </div>
    </div>
  </div>
</template>

<script>
import UserCard from './components/UserCard.vue';
import UserModal from './components/UserModal.vue';

export default {
  name: 'App',
  components: {
    UserCard,
    UserModal,
  },
  data() {
    return {
      users: [],
      newUser: {
        name: '',
        age: null,
        city: '',
        email: '',
      },
      searchQuery: '',
      isModalOpen: false,
    };
  },
  computed: {
    filteredUsers() {
      return this.users.filter(user =>
        user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
  methods: {
    openModal() {
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
    },
    addUser() {
      this.users.push({ ...this.newUser });
      this.saveUsersToLocalStorage();
      this.newUser = { name: '', age: null, city: '', email: '' };
      this.closeModal();
    },
    saveUsersToLocalStorage() {
      localStorage.setItem('users', JSON.stringify(this.users));
    },
    loadUsersFromLocalStorage() {
      const users = localStorage.getItem('users');
      if (users) {
        this.users = JSON.parse(users);
      }
    },
  },
  mounted() {
    this.loadUsersFromLocalStorage();
  },
};
</script>

<style>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  position: relative;
  max-width: 500px;
  width: 100%;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: none;
  border: none;
  font-size: 1.5em;
  cursor: pointer;
}
</style>