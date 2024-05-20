<template>
    <div>
      <h5>Create New Post</h5>
      <div class="form-group">
        <label for="postTitle">Title</label>
        <input type="text" class="form-control" v-model="newPost.title" id="postTitle" placeholder="Enter title">
      </div>
      <div class="form-group">
        <label for="postBody">Body</label>
        <textarea class="form-control" v-model="newPost.body" id="postBody" rows="3" placeholder="Enter body"></textarea>
        <button class="btn btn-primary" @click="createPost">Create Post</button>
      </div>
      <div class="form-group">
        <label for="postUser">User</label>
        <select class="form-control" v-model="newPost.userId" id="postUser" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
  
      <hr>
      <h5>Posts</h5>
      <div v-if="loadingPosts" class="text-muted">Loading posts...</div>
      <div v-else>
        <PostItem v-for="post in posts" :key="post.id" :post="post" :users="users" />
      </div>
    </div>
  </template>
  
  <script>
  import PostItem from './PostItem.vue'
  
  export default {
    components: {
      PostItem
    },
    data() {
      return {
        newPost: {
          title: '',
          body: '',
          userId: null
        },
        posts: [],
        users: [],
        loadingPosts: false
      }
    },
    methods: {
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
      createPost() {
        fetch('https://jsonplaceholder.typicode.com/posts', {
          method: 'POST',
          body: JSON.stringify(this.newPost),
          headers: {
            'Content-type': 'application/json; charset=UTF-8',
          },
        })
        .then(response => response.json())
        .then(data => {
          console.log('Post created successfully:', data);
          this.posts.unshift(data);
          this.newPost.title = '';
          this.newPost.body = '';
          this.newPost.userId = null;
        })
        .catch(error => {
          console.error('Error creating post:', error);
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
      }
    },
    mounted() {
      this.fetchUsers();
    }
  }
  </script>
  