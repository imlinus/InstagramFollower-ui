<template>
  <section class="section container">
    <div class="columns">
      <div v-show="!response" class="column is-half is-offset-one-quarter">
        <div class="block">
          <h3 class="title is-3">Instagram Follower</h3>
          <p>This is a tool that will help you follow people that have liked a post, spread out over the day in order to not get banned.</p>
        </div>

        <div class="field">
          <label class="label">Username</label>

          <div class="control">
            <input class="input" type="text" v-model="username" />
          </div>
        </div>

        <div class="field">
          <label class="label">Password</label>

          <div class="control">
            <input class="input" type="password" v-model="password" />
          </div>
        </div>

        <div class="field">
          <label class="label">Link to post</label>

          <div class="control">
            <input class="input" type="text" v-model="url" />
          </div>
        </div>

        <div class="field">
          <button class="button is-primary" @click="getUsersWhoLikedPost">
            Go
          </button>
        </div>
      </div>

      <div v-if="response">
        {{ response }}
      </div>
    </div>
  </section>
</template>

<script lang="js">
export default {
  data () {
    return {
      hasResponse: false,
      hasError: false,
      username: '',
      password: '',
      url: '',
      response: null
    }
  },

  computed: {
    postId () {
      const postId = this.url.split('.com/p/')[1] // last part of the url
      return postId.replace(/\/$/, '') // remove trailing slash
    }
  },

  methods: {
    async getUsersWhoLikedPost () {
      try {
        const response = await fetch('https://instagram-follower-backend.vercel.app/likes', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            usr: this.username,
            pwd: this.password,
            postId: this.postId
          })
        })

        const users = await response.json()

        console.log(users)
        
        users.forEach(user => {
          setTimeout(() => {
            this.followUser(user.ID)
           }, 5000)
        ))
      } catch (error) {
        this.hasError = true
      }
    },
    
    followUser (userId) {
      try {
        const response = await fetch('https://instagram-follower-backend.vercel.app/follow', {
          method: 'POST',
          headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            usr: this.username,
            pwd: this.password,
            usrId: id
          })
        })

        const data = await response.json()

        console.log('Response', data)
      } catch (error) {
        console.error(error)
      }
    }  
  }
}
</script>
