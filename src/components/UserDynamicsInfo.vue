<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3 img-field">
                    <img class="img-fluid" :src="user.photo" alt="头像">
                </div>
                <div class="col-9">
                    <div class="name">{{ user.username }}</div>
                    <div class="fan">粉丝: {{ user.followerCount }}</div>
                    <button @click="follow" v-if="!is_me && !user.is_followed" type="button"
                        class="btn btn-secondary btn-sm">+关注</button>
                    <button @click="unfollow" v-if="!is_me && user.is_followed" type="button"
                        class="btn btn-secondary btn-sm">已关注</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { useStore } from 'vuex';
import $ from 'jquery';

export default {
    name: "UserDynamicsInfo",
    props: {
        user: {
            type: Object,
            required: true,
        },
        is_me: {
            type: Boolean,
            required: true,
        }
    },
    setup(props, context) {
        const store = useStore();

        const follow = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id: props.user.id,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit("follow");
                    }
                }
            });
        };

        const unfollow = () => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id: props.user.id,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit("unfollow");
                    }
                }
            });
        };

        return {
            follow,
            unfollow,
        }
    }
}
</script>

<style scoped>
img {
    border-radius: 50%;
}

.name {
    font-weight: bolder;
}

.fan {
    font-size: 12px;
    color: gray;
}

button {
    padding: 2px 4px;
    font-size: 12px;
}

.img-field {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
</style>