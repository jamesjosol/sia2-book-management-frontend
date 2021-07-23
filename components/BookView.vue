<template>
    <div>
        <b-modal id="deleteBookModal" title="Delete Book?" header-bg-variant="warning" centered size="sm" button-size="sm" ok-title="Delete Book" ok-variant="danger" headerClass="p-2 border-bottom-0" footerClass="p-2 mt-2 border-top-0" @ok="onDelete">
            Are you sure you want to delete <b>{{ book.title }}</b> <i class="fas fa-exclamation-triangle"></i>
        </b-modal>
        <NoticeMessageModal :message="noticeMsg" :title="noticeTitle" :titleVariant="noticeTitleVariant" />
        <button class="btn btn-success float-right mb-1" @click="onNew">
            <i class="fa fa-book"></i><i class="fa fa-plus small-icon"></i> New Book
        </button>
        <h4>Book View</h4>

        <hr>

        <form action="" @submit.prevent="onSave" class="text-white">
            <b-form-group label="Title">
                <b-form-input v-model="book.title"> </b-form-input>
            </b-form-group>

            <b-form-group label="Description">
                <b-form-textarea id="description" v-model="book.description" trim rows="3" cols="50"> </b-form-textarea>
            </b-form-group>

            <b-form-group label="Genre">
                <b-form-select id="genre" v-model="book.genre" :options="genres"></b-form-select>
            </b-form-group>

            <b-form-group label="Author">
                <b-form-input v-model="book.author"> </b-form-input>
            </b-form-group>

            <b-form-group label="Date Published">
                <b-form-input type="date" v-model="book.pub_date"> </b-form-input>
            </b-form-group>

             <b-form-group>
                <button class="btn btn-primary" type="submit"><i class="fas fa-save"></i> Save Changes</button>
                <button class="btn btn-danger" type="button" @click="confirmDelete" v-if="book.id"><i class="fas fa-trash"></i> Delete</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        book: {},
    },
    data() {
        return {
            genres: [
                {value: 'Art', text: 'Art'},
                {value: 'Anthologies', text: 'Anthologies'},
                {value: 'Biography', text: 'Biography'},
                {value: 'Comics', text: 'Comics'},
                {value: 'Fantasy', text: 'Fantasy'},
                {value: 'Fiction', text: 'Fiction'},
                {value: 'History', text: 'History'},
                {value: 'Horror', text: 'Horror'},
                {value: 'Music', text: 'Music'},
                {value: 'Mystery', text: 'Mystery'},
                {value: 'Romance', text: 'Romance'},
                {value: 'Science', text: 'Science'},
                {value: 'Thriller', text: 'Thriller'},
            ],
            noticeMsg: '',
            noticeTitle: '',
            noticeTitleVariant: '',
        }
    },
    methods: {
        async onSave() {
            try {
                if(!this.book.id) {
                    await this.$axios.post('/api/books', this.book)
                    this.noticeTitle = "New Book Added"
                    this.noticeMsg = this.book.title + " successfully added."
                }else {
                    await this.$axios.put('/api/books/' + this.book.id, this.book)
                    this.noticeTitle = "Book Updated"
                    this.noticeMsg = this.book.title + " has been updated."
                }
                this.noticeTitleVariant = "success"

                this.$emit('saved')
            }catch(err) {
                // alert(err.response.data.message)
                this.noticeTitle = "Invalid Fields"
                this.noticeTitleVariant = "danger"
                this.noticeMsg = err.response.data.message
            }
            this.$bvModal.show('NoticeMsgModal')
            setTimeout(() => {
                this.$bvModal.hide('NoticeMsgModal')
            }, 2 * 1000) 
        },
        onNew() {
            this.$emit('newBook')
        },
        async onDelete() {
            try {
                await this.$axios.delete('/api/books/' + this.book.id)
                this.noticeTitle = "Book Deleted"
                this.noticeTitleVariant = "success"
                this.noticeMsg = this.book.title + " has been deleted."
                this.$emit('deleted')
            }catch(err) {
                // alert(err.response.data.message)
                this.noticeTitle = "Error"
                this.noticeTitleVariant = "danger"
                this.noticeMsg = err.response.data.message
            }
            this.$bvModal.show('NoticeMsgModal')
            setTimeout(() => {
                this.$bvModal.hide('NoticeMsgModal')
            }, 2 * 1000) 
        },
        confirmDelete() {
            this.$bvModal.show('deleteBookModal')
            
        }
    }
}
</script>

<style scoped>
.small-icon{
    font-size: 0.7em;
    margin-left: -2px;
}
</style>