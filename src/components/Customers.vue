<template>
    <Page>
        <ActionBar title="Customers">
            <ActionItem @tap="onTapLogout" ios.systemIcon="9" ios.position="right" text="Logout" android.position="popup" />
            <SearchBar height="50px" width="100%" hint="Search Customer" v-model="searchQuery" @textChange="searchCustomer"/>
        </ActionBar>
        <ListView for="customer in filteredCustomers"  @itemTap="onTap" separatorColor="blue">
            <v-template>
                <FlexboxLayout flexDirection="column">
                    <Label class="list-item" :text="customer.name.toUpperCase()" marginLeft="10px"/>
                    <Label class="sub-item" :text="`customerID: ${customer.id}`" marginLeft="10px"/>
                    <Label class="sub-item" :text="`Phonenumber: ${customer.phone_number}`" marginLeft="10px"/>
                    <Label class="sub-item" :text="`City: ${customer.city}`" marginLeft="10px"/>
                    <Label class="sub-item" :text="`Location: ${customer.location}`" marginLeft="10px"/>
                    <Label class="sub-item" :text="`Region: ${customer.region}`" marginLeft="10px"/>
                    <Label class="sub-item" :text="`Coordinates: ${customer.coordinates}`" marginLeft="10px"/>
                </FlexboxLayout>
            </v-template>
        </ListView>
    </Page>
</template>


<script>
import util from "../utils/utils";
import axios from "axios";
import Loans from "./Loans";
import App from "./App";

export default {
    name: "Customers",
    components: {
        Loans,
        App
    },
    data(){
        return {
            searchQuery: "",
            customers: []
        }
    },
    methods: {
        searchCustomer(){
                axios.get(`${util.API_URL}/customers?name=${this.searchQuery.trim()}`)
                .then((res) => {
                    this.customers = res.data
                })
                .catch((err) => {
                    console.error(err);
                });
        },
        onTap(event){
            
            this.$navigateTo(Loans, {
                transition: {},
                transitioniOS: {},
                transitionAndroid: {},
                props: {
                    customer: event.item
                }
            });
        },
        goBack(event){
            this.$navigateTo(App);
        },
        onTapLogout(event){
            this.$navigateTo(App);
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
        axios.get(`${util.API_URL}/customers`)
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
.list-item {
    font-size: 30px;
    font-weight: 500;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
.stretch {
    width: 100%;
}
</style>