<template>
    <ContentBase>
        <div class="row">
          <div class="col-3">
            <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user"/>
            <UserProfileWrite v-if="is_me" @post_a_post="post_a_post"/>
          </div>
          <div class="col-9">
            <UserProfilePosts :user="user" :post="post" @delete_a_post="delete_a_post"/>
          </div>
        </div>
    </ContentBase>
</template>
  
<script>
    import { reactive } from "vue";
    import { useRoute } from "vue-router";
    import ContentBase from "../components/ContentBase.vue";
    import UserProfileInfo from "../components/UserProfileInfo.vue";
    import UserProfilePosts from "../components/UserProfilePosts.vue";
    import UserProfileWrite from "../components/UserProfileWrite.vue";
    import $ from "jquery";
    import { useStore } from "vuex";
    import { computed } from "vue";

    export default {
      name : 'UserProfileView',
      components : {
        ContentBase,
        UserProfileInfo,
        UserProfilePosts,
        UserProfileWrite,
      },
      setup() {
        const store = useStore();
        const route = useRoute();
        console.log(route.params.userId);
        const userId = parseInt(route.params.userId);
        const user = reactive({ });
        const post = reactive({ });

        $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
                type: "GET",
                data: {
                    user_id: userId,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,
                },
                success(resp){
                    //console.log(resp);
                    user.id = resp.id;
                    user.username = resp.username;
                    user.photo = resp.photo;
                    user.followerCount = resp.followerCount;
                    user.is_followed = resp.is_followed;
                },
        });

        $.ajax({
          url: "https://app165.acapp.acwing.com.cn/myspace/post/",
          type: "GET",
          data: {
             user_id : userId,
          },
          headers: {
                'Authorization': "Bearer " + store.state.user.access,
          },
          success(resp){
            post.posts = resp;
            post.count = resp.length;
          }
        });
        const follow = () => {
          if(user.is_followed)  return;
          user.is_followed = true;
          user.followerCount ++; 
        };
        const unfollow = () => {
          if(!user.is_followed) return;
          user.is_followed = false;
          user.followerCount --;
        };
        const post_a_post = (content) => {
          post.count++;
          post.posts.unshift({
              id : post.count,
              userId : 1,
              content : content,
          })
        };
        const delete_a_post = post_id => {
          post.posts = post.posts.filter(post => post.id !== post_id);
          post.count = post.posts.length;
        }
        const is_me = computed(() => userId === store.state.user.id );
        return {
          user,
          post,
          follow,
          unfollow,
          post_a_post,
          is_me,
          delete_a_post,
        }
      }
    }
</script>

<style scoped>
</style>