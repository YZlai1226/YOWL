-<template>
    <div class="dashboard">
        <UserSettings/>
        <div class="postLinks">
          <p>POSTS</p>
          <p>COMMENTS</p>
          <p>UPVOTES</p>
        </div>
         <PostManager v-if="postsFiltered.length !== 0" :postsFiltered="postsFiltered" @deletePost="deletePost" :user="user" :users="users" :communities="communities" :comments="comments"
         @ReportPost="ReportPost"/>
                 <!--<UpVotedPostsManager/> -->

    </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import UserSettings from '@/components/SpecialComponents/UserSettings.vue'
import PostManager from '@/components/SharedComponents/PostManager.vue'
import { mapGetters } from 'vuex'
// import UpVotedPostsManager from '@/components/SharedComponents/UpVotedPostsManager.vue'

export default {
  name: 'AdminView',
  components: {
    UserSettings,
    PostManager
    // UpVotedPostsManager
  },
  data () {
    return {
      postsFiltered: [],
      comments: [],
      users: [],
      communities: [],
      user: {},
      userId: 1
    }
  },
  computed: {
    ...mapGetters(['isLoggedIn', 'getUser'])
  },
  methods: {
    // deletePost (postId) {
    //   axios.delete('https://yowlteam.herokuapp.com/api/posts/' + postId)
    //     .then((response) => {
    //       axios.get('https://yowlteam.herokuapp.com/api/posts/user/' + this.userId)
    //         .then((response) => {
    //         // console.log('posts is', response.data)
    //           this.postsFiltered = response.data
    //         // console.log('post filtered is', this.postsFiltered)
    //         })
    //         .catch(error => console.log(error))
    //     })
    // },
    // ReportPost (postId) {
    //   console.log('in report post post id is ', postId)
    //   axios.put('https://yowlteam.herokuapp.com/api/posts/' + postId,
    //     {
    //       is_reported: 1
    //     })
    // }
  },
  created () {
    this.user = this.getUser
    this.userId = this.user.id
    axios.get('https://yowlteam.herokuapp.com/api/posts/user/' + this.userId)
      .then((response) => {
        // console.log('posts is', response.data)
        this.postsFiltered = response.data
        console.log('post filtered is', this.postsFiltered)
      })
      .catch(error => console.log(error))
    axios
      .get('https://yowlteam.herokuapp.com/api/communities')
      .then((response) => {
        this.communities = response.data
      })
      .catch((error) => console.log(error))
    axios
      .get('https://yowlteam.herokuapp.com/api/comments')
      .then((response) => {
        this.comments = response.data
      })
      .catch((error) => console.log(error))
    axios.get('https://yowlteam.herokuapp.com/api/users')
      .then((response) => {
        this.users = response.data
      })
      .catch(error => console.log(error))
  }
}
</script>

<style scoped>
.postLinks{
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;

}
.postLinks p{
  margin-right: 13px;
  font-size: 25px;
  cursor: pointer;
}
.postLinks p:hover{
  text-decoration: underline;
  color: rgb(14, 164, 233);

}
.dashboard{
  margin-left: 25%;
}

</style>
