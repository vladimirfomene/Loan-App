<template>
    <Page>
        <ActionBar title="Welcome to Loan App!"/>
        <FlexboxLayout justifyContent="center" alignItems="center" marginTop="30%" backgroundColor="#3c495e">
            <TextField v-model="phoneNumber" />
            <TextField v-model="password" />
            <Button text="Login" @tap="login" />
        </FlexboxLayout>
    </Page>
</template>


<script>
import { PHONE_NUMBER, PASSWORD } from "../utils/utils";
import Customers from "./Customers.vue";

export default {
    name: "Login",
    data(){
        return {
            phoneNumber: "",
            password: ""
        }
    },
    methods: {
        login(){
            if(PHONE_NUMBER === this.phoneNumber && PASSWORD === this.password){
                this.$navigateTo(Customers);
            }
        }
    }
}
</script>

<style scoped>
@import "../../node_modules/bulma/css/bulma.css";
</style>