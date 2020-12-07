<template>
    <Page>
        <ActionBar title="Customers"/>
        <SearchBar v-model="searchQuery" @textChange="searchCustomer"/>
        <ListView for="customer in filteredCustomers" @itemTap="customerTap(customer)">
            <v-template>
                <Label :text="customer.name" />
            </v-template>
        </ListView>
    </Page>
</template>


<script>
import { API_URL } from "../utils/utils";
import axios from "axios";
import Profile from "./Profile.vue";

export default {
    name: "Login",
    data(){
        return {
            searchQuery: "",
            customers: []
        }
    },
    methods: {
        searchCustomer(){
                axios.get(`${API_URL}/customers?name=${searchQuery}`)
                .then((res) => {
                    this.customers = res.data
                })
                .catch((err) => {
                    console.error(err);
                });
        },
        customerTap(customer){
            this.$navigateTo(Profile, {
                transition: {},
                transitioniOS: {},
                transitionAndroid: {},

                props: {
                    customer
                }
            })
        }
    },
    computed: {
        filteredCustomers() {
            return this.customers.filter(customer => {
                return customer.name.toLowerCase().includes(this.searchQuery.toLowerCase())
            })
        }
    },
    created(){
        axios.get(`${API_URL}/customers`)
        .then((res) => {
            this.customers = res.data;
        })
        .catch((err) => {
            console.error(err);
        })
    }
}
</script>

<style scoped>
@import "../../node_modules/bulma/css/bulma.css";
</style>