<template>
    <Page>
        <ActionBar title="Welcome to Loan App!"/>
        <FlexboxLayout justifyContent="center" flexDirection="column" alignItems="center" marginTop="5%">
            <TextField v-model="phoneNumber" backgroundColor="#f9f9f9" maxLength="17" hint="Enter Your Phonenumber" marginBottom="2%"/>
            <TextField v-model="password" backgroundColor="#f9f9f9" maxLength="17" hint="Enter Your Password" marginBottom="2%"/>
            <Button text="Login" @tap="login" />
        </FlexboxLayout>
    </Page>
</template>

<script >
import util from "../utils/utils";
import Customers from "./Customers";

export default {
    name: "Login",
    components: {
      Customers
    },
    data(){
        return {
            phoneNumber: "",
            password: ""
        }
    },
    methods: {
        login(){
            //util.PHONE_NUMBER === String(this.phoneNumber).trim() && util.PASSWORD === String(this.password).trim()
            if(true){
                this.$navigateTo(Customers);
            }
        }
    }
}
</script>

<style scoped>
</style>
