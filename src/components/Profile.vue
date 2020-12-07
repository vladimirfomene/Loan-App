<template>
    <Page>
        <ActionBar title="Profile / Loans"/>
        <TabView :selectedIndex="selectedIndex" @selectedIndexChange="indexChange">
            <TabViewItem title="Profile">
                <table class="table">
                    <tr>
                        <th>
                            Customer ID
                        </th>
                        <td>
                            {{ customer.id }}
                        </td>
                    </tr>
                    <tr>
                        <th>
                            Name
                        </th>
                        <td>
                            {{ customer.name }}
                        </td>
                    </tr>
                    <tr>
                        <th>
                            Phone Number
                        </th>
                        <td>
                            {{ customer.phone_number }}
                        </td>
                    </tr>
                    <tr>
                        <th>
                            City
                        </th>
                        <td>
                            {{ customer.city }}
                        </td>
                    </tr>
                    <tr>
                        <th>
                            Location
                        </th>
                        <td>
                            {{ customer.location }}
                        </td>
                    </tr>
                    <tr>
                        <th>
                            Region
                        </th>
                        <td>
                            {{ customer.region }}
                        </td>
                    </tr>
                    <tr>
                        <th>
                            Coordinates
                        </th>
                        <td>
                            {{ customer.coordinates }}
                        </td>
                    </tr>
                </table>
            </TabViewItem>
            <TabViewItem title="Loans">
                <ScrollView>
                    <StackLayout>
                        <table class="table" v-for="loan in loans">
                            <tr>
                                <th>
                                    Created
                                </th>
                                <td>
                                    {{ loan.created_at }}
                                </td>
                            </tr>
                            <tr>
                                <th>
                                    Loan Amount
                                </th>
                                <td>
                                    {{ loan.loan_amount }}
                                </td>
                            </tr>
                            <tr>
                                <th>
                                    Amount Repaid
                                </th>
                                <td>
                                    {{ loan.amount_repaid }}
                                </td>
                            </tr>
                            <tr v-if="isArrear(loan)">
                                <th>
                                    Arrears
                                </th>
                                <td>
                                    {{ computeArrear(loan) }}
                                </td>
                            </tr>
                        </table>
                    </StackLayout>
                </ScrollView>
            </TabViewItem>
        </TabView>
    </Page>
</template>

<script>

import axios from "axios";
import { API_URL } from "../utils/utils";

export default {
    name: "Profile",
    props: ['customer'],
    data(){
        return {
            loans: []
        }
    },
    methods: {
        indexChange(args) {
            let newIndex = args.value
            console.log('Current tab index: ' + newIndex)
        },
        monthDiff(d1, d2) {
            let months = (d2.getFullYear() - d1.getFullYear()) * 12;
            months -= d1.getMonth();
            months += d2.getMonth();
            return months <= 0 ? 0 : months;
        },
        isArrear(loan){
            return loan.amount_repaid < Math.abs(monthDiff(new Date(loan.created_at), new Date()));
        },
        computeArrear(loan){
            return Math.abs(monthDiff(new Date(loan.created_at), new Date())) - loan.amount_repaid;
        },
        computePrepay(loan){
            return loan.amount_repaid - Math.abs(monthDiff(new Date(loan.created_at), new Date()));
        },
        isPrepay(loan){
            return loan.amount_repaid > Math.abs(monthDiff(new Date(loan.created_at), new Date()));
        }
    },
    mounted(){
        axios.get(`${API_URL}/loans?customerId=${this.customer.id}`)
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
@import "../../node_modules/bulma/css/bulma.css";
</style>