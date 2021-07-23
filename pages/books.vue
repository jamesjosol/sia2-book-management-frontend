<template>
    <div>
        <NavBar />
        
        <div class="container">
            <h1 class="mt-1">My Books</h1>
            <div class="row pb-3">
                <div class="col-7">
                    <h4>List of Books</h4>
                    <ul class="list-group custom-list-group">
                        <li class="list-group-item list-group-item-action btn-li" v-for="book in books" :key="book.id" @click="onSelected(book)">
                            {{ book.title }} <span class="float-right">by: {{ book.author }}</span>
                        </li>
                    </ul>

                </div>
                <div class="col-5 pt-2">
                    <div class="form-card">
                        <BookView :book="selectedBook" @saved="onChanges" @newBook="onNew" @deleted="onChanges"/>
                    </div>

                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
export default {
    data() {
        return {
            books: [],
            selectedBook: {}
        }
    },
    methods: {
        async getMyBooks() {
            await this.$axios.get('/api/books')
            .then((res) => {
                if(res.status == 200) {
                    this.books = res.data
                }
            })
        },
        onChanges() {
            this.getMyBooks()
            this.selectedBook = {}
        },
        onSelected(book) {
            this.selectedBook = book
        },
        onNew() {
            this.selectedBook = {}
        },
    },
    created() {
        this.getMyBooks()
    }
}
</script>

<style scoped>
.btn-li {
    color: #b3b3b3;
    
    cursor: pointer;
    background-color: #25252b;
}

.btn-li:hover {
    background-color: #2e2e36;
}

.btn-li:active {
    background-color: #2c2c33;
}
</style>