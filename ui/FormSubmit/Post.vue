<template>
    <div class="container mt-5 pt-5" style="min-height: 100vh">
        <div class="row">
            <div class="col-lg-6 col-12 mx-auto">
                <h2>Post</h2>
                <ul class="list-group mt-3">
                    <li class="list-group-item list-group-item-light p-3">
                        <h2>{{ post.title }}</h2>
                        Posted on {{ formatDate(post.created_at) }}
                    </li> 
                    <li class="list-group-item">
                        <p>{{ post.content }}</p>
                    </li> 
                    
                </ul>

                    <h2 class="mt-4 pt-4">Comments</h2>
                <table class="table table-bordered table-hover">
                    <thead class="thead-dark">
                        <tr>
                            <th>Content</th>
                            <th>Created Date</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="comment in comments" :key="comment.id" class="cursor-pointer">
                            <td>{{ comment.content }}</td>
                            <td>{{ formatDate(comment.created_at) }}</td>
                        </tr>
                    </tbody>
                </table>

                        
                    <div class="mt-4 pt-4">
                    <form @submit.prevent="createNewComment" class="mt-4">
                        <div class="form-group">
                            <label for="title">Add Comment</label>
                            <input type="text" class="form-control-bna" id="title" v-model="newComment.content" required>
                        </div>


                        <div class="mt-2">
                            <button type="submit" class="btn-bna">Add Comment</button>
                        </div>
                    </form>
                </div>

              
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';

export default{
    data(){
        return{
            post: [],
            comments: [],
            newComment: {
                content: ''
            }
        }
    },

    computed: {
        postId() {
            return this.$route.params.postId
        }
    },

    created(){
        this.fetchPost();
        this.fetchComments();
    },

    methods:{
        fetchPost(){
            axios.get(`http://localhost:8000/api/posts/${this.postId}`)
                .then((response) => {
                    this.post = response.data;
                })
                .catch((error) => {
                    console.error(error);
                });
        },
        formatDate(date) {
            return new Date(date).toLocaleDateString('en-GB');

        },
        fetchComments(){
            axios.get(`http://localhost:8000/api/posts/${this.postId}/comments`)
                .then((response) => {
                    this.comments = response.data;
                })
                .catch((error) => {
                    console.error(error);
                });
        },


        createNewComment(){
            axios.post(`http://localhost:8000/api/posts/${this.postId}/comments`, {
                content: this.newComment.content,
                post_id: this.postId
            })
            .then((response) => {
                this.comments.push(response.data);
                this.newComments.content = '';
            })
            .catch((error) => {
                console.log(error)
            })
        }
    }
}
</script>
<style scoped>
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
