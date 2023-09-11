<template>
    <div class="container mt-5 pt-5 " style="min-height: 100vh">
        <div class="row">
            <div class="col-12 col-lg-6 mx-auto">
                <h2 class="mb-4">Posts</h2>
                <table class="table table-bordered table-hover">
                    <thead class="thead-dark">
                        <tr>
                            <th>Title</th>
                            <th>Created Date</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="post in posts" :key="post.id" @click="viewPost(post.id)" class="cursor-pointer">
                            <td class="col-7">{{ post.title }}</td>
                            <td class="col-5">{{ formatDate(post.created_at) }}</td>
                        </tr>
                    </tbody>
                </table>

                <div class="mt-4 pt-4">
                    <form @submit.prevent="createNewPost" class="mt-4">
                        <div class="form-group">
                            <label for="title">Title</label>
                            <input type="text" class="form-control-bna" id="title" v-model="newPost.title" required>
                        </div>

                        <div class="form-group">
                            <label for="title">Content</label>
                            <textarea class="form-control-bna" id="content" v-model="newPost.content" required></textarea>
                        </div>

                        <div class="mt-2">
                            <button type="submit" class="btn-bna">Create Post</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default {

    data() {
        return {
            posts: [],
            newPost: {
                title: '',
                content: '',
            }
        }
    },
    computed: {
        categoryId() {
            return this.$route.params.categoryId
        }
    },
    created() {
        this.fetchPosts();
    },
    methods: {
        fetchPosts() {
            axios.get('http://localhost:8000/api/posts')
                .then((response) => {
                    this.posts = response.data;
                })
                .catch((error) => {
                    console.error(error);
                });
        },

        formatDate(date) {
            return new Date(date).toLocaleDateString('en-GB');

        },

        createNewPost() {
            axios.post('http://localhost:8000/api/posts', {
                title: this.newPost.title,
                content: this.newPost.content,
                category_id: this.categoryId
            })
            .then((response) => {
                this.posts.push(response.data);
                this.newPost.title = '';
                this.newPost.content = '';
            })
            .catch((error) => {
                console.log(error);
            })
        },
        viewPost(postId) {
            this.$router.push(`/projects/appform-categories/${this.categoryId}/posts/${postId}`)
        }
    }
}
</script>
<style>
.form-control-bna{
    display: block;
    width: 100%;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    line-height: 1.5rem;
    /* color: #000000; */
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.form-control-bna:focus {
    color: #000000;
    background-color: #fff;
    border-color: #448c74;
    outline: 0;
    box-shadow: 0 0 0 0.25rem #ecf3f1;
}
.form-control-bna::placeholder {
    color: #000000;
    opacity: 1;
}
.form-control-bna:disabled {
    background-color: #e9ecef;
    opacity: 1;
}

</style>