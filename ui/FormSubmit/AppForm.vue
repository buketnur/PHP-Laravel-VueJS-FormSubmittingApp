<template>
    <div class="container mt-5 pt-5" style="min-height: 100vh">
        <div class="row">
            <div class="col-lg-6 col-12 mx-auto">
                <h2>Categories</h2>
                <ul class="list-group mt-3 ">
                    <li v-for="category in categories" :key="category.id" class="list-group-item d-flex justify-content-between align-items-center">          
                    <router-link class="btn-bna-link-3 " tag="a" :to="'/projects/appform-categories/' + category.id"> <span>{{ category.name }}</span></router-link> 
                    <button type="submit" class="btn-bna-small" @click="deleteCategory(category.id)"> x </button>            
                    </li>
                </ul>

                <div class="mt-4 pt-4">
                    <form @submit.prevent="createNewCategory" class="mt-4">
                        <div class="form-group">
                            <label for="title">Name</label>
                            <input type="text" class="form-control-bna" id="title" v-model="newCategory.name" required>
                        </div>


                        <div class="mt-2">
                            <button type="submit" class="btn-bna" >Create Category</button>
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
            categories: [],
            newCategory: {
                name: '',
            }
        }
    },

    beforeMount(){
        this.fetchCategories();
    },
    created(){
        this.fetchCategories();
    },

    methods:{
        fetchCategories(){
            axios.get('http://localhost:8000/api/categories')
                .then((response) => {
                    this.categories = response.data;
                })
                .catch((error) => {
                    console.error(error);
                });
        },
        createNewCategory(){
            axios.post('http://localhost:8000/api/categories',{
                name: this.newCategory.name
            })
            .then((response) => {
                this.categories.push(response.data);
                this.newCategory.name = '';
            })
            .catch((error) => {
                console.log(error);
            })
                
        },

        deleteCategory(categoryId) {
        axios.delete(`http://localhost:8000/api/categories/${categoryId}`)
            .then((response) => {
                const index = this.categories.findIndex(category => category.id === categoryId);
                if (index !== -1) {
                    this.categories.splice(index, 1);
                }
            })
            .catch((error) => {
                console.error(error);
            });
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
