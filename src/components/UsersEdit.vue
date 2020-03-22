<template>
    <div class="users-form">
        <form action="" @submit="addUser">
            <b-field :label="Labels.NAME">
                <b-input required v-model="userParams.firstName"></b-input>
            </b-field>
            <b-field :label="Labels.SURNAME">
                <b-input required v-model="userParams.surName"></b-input>
            </b-field>
            <b-field :label="Labels.PHONE">
                <b-input required v-model="userParams.phone"></b-input>
            </b-field>
            <b-field :label="Labels.EMAIL">
                <b-input required v-model="userParams.email"></b-input>
            </b-field>
            <div class="controls">
                <b-button class="save-user" type="is-success" native-type="submit">Save</b-button>
                <router-link to="/">
                    <b-button type="is-danger">Cancel</b-button>
                </router-link>
            </div>
        </form>
        <form action="" @submit="addUsersList" v-if="this.$route.params.id === undefined">
            <b-field :label="Labels.JSON_LIST">
                <b-input class="json-textarea" required type="textarea" v-model="jsonTextarea"></b-input>
            </b-field>            
            <div class="controls">
                <b-button class="save-user" type="is-success" native-type="submit">Save</b-button>
                <router-link to="/">
                    <b-button type="is-danger">Cancel</b-button>
                </router-link>
            </div>
        </form>
    </div>
</template>

<script>
    import {mapState ,mapGetters} from "vuex";
    import Labels from "../enums/labels-name-enum";

    export default {
        data() {
            return {
                id: this.$route.params.id,
                userParams: null,
                Labels,
                jsonTextarea: {}                
            }
        },
        
        computed: {
            ...mapState({
                state: (state) => state.users
            }),
            ...mapGetters({
                user: `getUser`
            }),

        },
        created() {
            this.userParams = Object.assign({}, this.user(this.id));            
        },
        mounted() {
            this.jsonTextarea = JSON.stringify(this.state, undefined, 2); 
            console.log(this.$route.params.id);
                       
        },
        methods: {
            addUser() {
                if (this.$route.params.id || this.$route.params.id === 0) {
                    this.$store.dispatch("UPDATE_USER", {index: this.id, user: this.userParams});
                } else  {
                    this.$store.dispatch("SET_USER", this.userParams);
                }
                this.$router.push("/");
            },
            addUsersList() {                
                this.jsonTextarea = JSON.parse(this.jsonTextarea);                
                this.$store.dispatch("UPDATE_USERS", this.jsonTextarea);
                this.$router.push("/"); 
            },
        }
    }
</script>

<style>
    .users-form {
        width: 500px;
        margin: 0 auto;
    }

    .save-user {
        margin-right: 10px;
    }

    .json-textarea .textarea {
        height: 200px;
        font-size: smaller;
    }
    
</style>