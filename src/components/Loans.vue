<template>
    <Page>
        <ActionBar title="Loans">
            <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="goBack" />
            <ActionItem @tap="onTapLogout" ios.systemIcon="9" ios.position="right" text="Logout" android.position="popup" />
        </ActionBar>
        <ListView for="loan in loans"  separatorColor="blue">
            <v-template>
                <FlexboxLayout flexDirection="column">
                    <Label class="list-item" :text="getLocaleStringDate(loan.created_at)" marginLeft="10px"/>
                    <Label class="sub-item" :text="`Loan Amount: ${loan.loan_amount}$`" marginLeft="10px"/>
                    <Label class="sub-item" :text="`Amount Repaid: ${loan.amount_repaid}$`" marginLeft="10px"/>
                    <Label class="sub-item" v-if="isArrear(loan)" :text="`Arrears: ${computeArrear(loan)}$`" marginLeft="10px"/>
                    <Label class="sub-item" v-if="isPrepay(loan)" :text="`Prepayments: ${computePrepay(loan)}$`" marginLeft="10px"/>
                    <Label class="sub-item" v-if="isArrear(loan) || isPrepay(loan)" :text="`Payment Score: ${getPaymentScore(loan)}%`" marginLeft="10px"/>
                </FlexboxLayout>
            </v-template>
        </ListView>
    </Page>
</template>

<script>
import axios from "axios";
import util from "../utils/utils";
import Customers from "./Customers";
import App from "./App";

export default {
    name: "Loans",
    props: ['customer'],
    components: {
        Customers,
        App
    },
    data(){
        return {
            loans: []
        }
    },
    methods: {
        getPaymentScore(loan){
            let expected_repay = 11 * loan.loan_amount;
            let gap_amount = 0;

            if(this.isArrear(loan)){
                gap_amount = this.computeArrear(loan);
            }

            if(this.isPrepay(loan)){
                gap_amount = this.computePrepay(loan);
            }

            return ((gap_amount / expected_repay) * 100).toFixed(2);
        },
        getLocaleStringDate(str){
            return new Date(str).toLocaleDateString();
        },
        monthDiff(d1, d2) {
            let diff =(d2.getTime() - d1.getTime()) / 1000;
            diff /= (60 * 60 * 24 * 7 * 4);
            return Math.abs(Math.round(diff));
        },
        isArrear(loan){
            if(loan.loan_amount === loan.amount_repaid) return false;
            return loan.amount_repaid < (Math.abs(this.monthDiff(new Date(loan.created_at), new Date())) * (loan.loan_amount / 12));
        },
        computeArrear(loan){
            return (Math.abs(this.monthDiff(new Date(loan.created_at), new Date())) * (loan.loan_amount / 12)) - loan.amount_repaid;
        },
        computePrepay(loan){
            return loan.amount_repaid - (Math.abs(this.monthDiff(new Date(loan.created_at), new Date())) * (loan.loan_amount / 12));
        },
        isPrepay(loan){
            if(loan.loan_amount === loan.amount_repaid) return false;
            return loan.amount_repaid > (Math.abs(this.monthDiff(new Date(loan.created_at), new Date())) * (loan.loan_amount / 12));
        },
        goBack(event){
            this.$navigateTo(Customers);
        },
        onTapLogout(event){
            this.$navigateTo(App);
        }
    },
    mounted(){
        axios.get(`${util.API_URL}/loans?customerId=${this.customer.id}`)
        .then((res) => {
            this.loans = res.data;
        })
        .catch((error) => {
            console.error(error);
        });
    }
}
</script>

<style scoped>
.list-item {
    font-size: 30px;
    font-weight: 500;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
}
</style>