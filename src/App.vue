<template>
  <div>
    <nav class="navbar">
      <button class="nav-button" @click="showTodos">Todos</button>
      <button class="nav-button" @click="showPosts">Posts</button>
      <button class="nav-button" @click="showAlbums">Albums</button>
    </nav>
    <div class="containerblue">
      <div v-if="activeTab === 'todos'" class="red">
        <div class="black">
          <h1> MY APP <span></span></h1>
        </div>
        <div class="list">
          <div class="li1">
            <div class="li2">
              <h2 class="addtitle">Add new todo...</h2>
              <input v-model="todo" type="text" class="box" @keyup.enter="add" />
            </div>
            <div>
              <button class="button-20" @click="add">ADD</button>
            </div>
          </div>
          <small class="titillium-web-regular">Total TODO : {{ totalTODO }}</small>
          <ul class="list-group mt-3">
            <li 
              class="list-group-item d-flex justify-content-between align-items-center"
              v-for="(item, index) in todos"
              :key="index"
              :class="{ 'completed': item.completed }"
            >
              <span v-if="!item.completed">{{ item.text }}</span>
              <span v-else class="done">{{ item.text }}</span>
              <div>
                <button class="button-29" @click="remove(index)">X</button>
                <button class="button-28" @click="toggleCompletion(index)">
                  {{ item.completed ? 'Undone' : 'Done' }}
                </button>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div v-if="activeTab === 'posts'" class="purple">
        <div class="blue">
          <h2>Posts</h2>
        </div>
        <div class="yellow">
          <div class="form-group">
            <label for="postUser" class="titillium-web-regular">User</label>
            <select class="box" v-model="newPost.userId" id="postUser" @change="fetchPosts">
              <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
            </select>
          </div>
          <hr>
          <div v-if="loadingPosts" class="listtext">Loading posts...</div>
          <div v-else>
            <div v-for="post in posts" :key="post.id" class="card mb-3">
              <div class="card-body">
                <h5 class="card-title">{{ post.title }}</h5>
                <p class="card-text">{{ post.body }}</p>
                <p class="card-text">User: {{ getUserById(post.userId).name }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="activeTab === 'albums'" class="purple">
        <div class="blue">
          <h2>Albums</h2>
        </div>
        <!-- Konten untuk albums bisa ditambahkan sesuai kebutuhan -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todo: '',
      todos: [],
      activeTab: 'todos',
      newPost: {
        title: '',
        body: '',
        userId: null
      },
      posts: [],
      users: [],
      loadingPosts: false
    };
  },
  computed: {
    totalTODO() {
      return this.todos.length;
    }
  },
  methods: {
    add() {
      if (this.todo.trim() !== '') {
        this.todos.push({ text: this.todo, completed: false });
        this.todo = '';
      }
    },
    remove(index) {
      this.todos.splice(index, 1);
    },
    toggleCompletion(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
    showTodos() {
      this.activeTab = 'todos';
    },
    showPosts() {
      this.activeTab = 'posts';
      this.fetchPosts();
    },
    showAlbums() {
      this.activeTab = 'albums';
      // Tambahkan operasi lain yang diperlukan saat tombol "Albums" ditekan
    },
    fetchPosts() {
      if (!this.newPost.userId) return; // Return if no user is selected
      this.loadingPosts = true;
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.newPost.userId}`)
        .then(response => response.json())
        .then(data => {
          this.posts = data;
          this.loadingPosts = false;
        })
        .catch(error => {
          console.error('Error fetching posts:', error);
          this.loadingPosts = false;
        });
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
          this.users = data;
        })
        .catch(error => {
          console.error('Error fetching users:', error);
        });
    },
    getUserById(id) {
      return this.users.find(user => user.id === id);
    }
  },
  mounted() {
    this.fetchUsers();
  }
};
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

form {
  display: flex;
  flex-direction: column;
}

li {
  list-style-type: none;
  display: flex;
  justify-content: space-between;
}

h1 {
  font-family: "Kanit", sans-serif;
  font-weight: 600;
  font-style: normal;
  font-size: 120px;
}

h1 span {
  margin: 40px;
}

.containerblue {
  background-image: url(./assets/2825710.gif);
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10%;
}

.red {
  display: flex;
  flex-direction: column;
  width: 40%;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.6);
  padding: 20px;
}

.purple {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  width: 40%;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.6);
  padding: 20px;
}

.black {
  position: relative;
  background-color: white;
  height: 15%;
  width: 100%;
  border-top-left-radius: 20px;
  overflow: hidden;
  margin-bottom: 20px;
}

.blue {
  background-color: black;
  color: white;
  height: 15%;
  width: 100%;
  border-top-right-radius: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  margin-bottom: 20px;
}

.yellow {
  height: 85%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  backdrop-filter: blur(10px);
  border-bottom-left-radius: 200px;
  background: rgba(255, 255, 255, 0.5);
  box-shadow: 0px 0px 16px 0px rgba(255, 255, 255, 0.5);
}

li {
  padding-top: 20px;
}

ul {
  height: 80%;
  overflow: auto;
}

.completed {
  text-decoration: line-through;
}

.button-28,
.button-29,
.button-20 {
  border: 2px solid #1A1A1A;
  border-radius: 15px;
  box-sizing: border-box;
  color: #3B3B3B;
  cursor: pointer;
  display: inline-block;
  font-family: Roobert, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  font-size: 16px;
  font-weight: 600;
  line-height: normal;
  margin: 0;
  min-width: 0;
  outline: none;
  padding: 0 24px;
  text-align: center;
  text-decoration: none;
  transition: all 300ms cubic-bezier(.23, 1, 0.32, 1);
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  will-change: transform;
}

.button-28:disabled,
.button-29:disabled,
.button-20:disabled {
  pointer-events: none;
}

.button-28:hover,
.button-29:hover,
.button-20:hover {
  color: #fff;
  background-color: #1A1A1A;
  box-shadow: rgba(0, 0, 0, 0.25) 0 8px 15px;
  transform: translateY(-2px);
}

.button-28:active,
.button-29:active,
.button-20:active {
  box-shadow: none;
  transform: translateY(0);
}

.box {
  position: relative;
  bottom: 15px;
  width: 100%;
  color: rgb(36, 35, 42);
  font-size: 16px;
  line-height: 20px;
  min-height: 28px;
  border-radius: 4px;
  padding: 8px 16px;
  border: 2px solid transparent;
  box-shadow: rgb(0 0 0 / 12%) 0px 1px 3px, rgb(0 0 0 / 24%) 0px 1px 2px;
  background: rgb(251, 251, 251);
  transition: all 0.1s ease 0s;
}

.box:focus {
  border: 2px solid rgb(124, 138, 255);
}

.navbar {
  display: flex;
  justify-content: center;
  background-color: #333;
  padding: 10px;
}

.nav-button {
  color: white;
  background-color: #333;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.nav-button:hover {
  background-color: #555;
}
</style>
