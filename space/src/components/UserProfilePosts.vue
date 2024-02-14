<template>
    <div class="card">
        <div class="card-body">             
            <div v-for="post in post.posts" :key="post.id">
                <div class="card">
                    <div class="card-body">
                        {{ post.content }}
                        <button @click="delete_a_post(post.id)" v-if="is_me" type="button" class="btn btn-danger btn-sm">删除</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {computed} from "vue";
    import { useStore } from "vuex";
    import $ from 'jquery';

    export default {
        name : 'UserProfilePosts',
        props : {
            post : {
                type : Object,
                requried : true,
            },
            user: {
                type: Object,
                requried: true,
            }
        },
        setup(props,context){//子类传父类用props
            const store = useStore();
            console.log(store.state.user.id,props.user.id);
            let is_me = computed(() => store.state.user.id === props.user.id );

            const delete_a_post = post_id => {
                $.ajax({
                    url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                    type: "DELETE",
                    data: {
                        post_id,
                    },
                    headers:{
                        'Authorization': 'Bearer ' + store.state.user.access,
                    },
                    success(resp){
                        if(resp.result === "success"){
                            context.emit('delete_a_post',post_id);
                        }
                    },
                });
            }
            return {
                is_me,
                delete_a_post,
            }
        }
    }
</script>

<style scoped>
button{
    float: right;
}
</style>