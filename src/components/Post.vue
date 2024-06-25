<template>
    <div class="purple">
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
</template>