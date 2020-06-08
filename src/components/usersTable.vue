<template>
    <v-card>
        <v-card-title>
            Users Table
            <v-spacer></v-spacer>
            <v-text-field
                    v-model="search"
                    append-icon="mdi-magnify"
                    label="Search"
                    single-line
                    hide-details
            ></v-text-field>
        </v-card-title>
        <v-data-table id="data_table"
                :headers="headers"
                :items="users"
                :search="search"
                :multi-sort=true
                no-data-text="No data"
                no-results-text="No results"
                hide-default-footer
                :page.sync="page"
                :items-per-page="itemsPerPage"
                @current-items="getFiltered"
                :dark=true

        ></v-data-table>
        <v-layout row wrap>
            <v-col class="d-flex" cols="12" sm="6">
                <v-text-field  v-if="this.search != ''"
                               v-model="affectedRows"
                               label="Affected Rows"
                               readonly
                               outlined>
                </v-text-field>
            </v-col>
            <v-col class="d-flex" cols="12" sm="6">
                <v-select :items="paginationValues"
                          :value="itemsPerPage"
                          label="Items per page"
                          item-text="name"
                          outlined
                          v-on:change= "itemsPerPage = parseInt($event, 10)"
                          item-value="value">
                </v-select>
            </v-col>
        </v-layout>
    </v-card>
</template>

<script>
    import axios from "axios";

    export default {
        name: "userTable",
        data() {
            return {
                page: 1,
                itemsPerPage: 10,
                search: '',
                headers: [
                    { text: '#', value: 'id' },
                    { text: 'FirstName', value: 'first_name', filterable: true, id: 'first_name'},
                    { text: 'LastName', value: 'last_name', filterable: true },
                    { text: 'UserName', value: 'user_name' , filterable: true },
                    { text: 'Age', value: 'age' , filterable: true},
                    { text: 'Gender', value: 'gender' , filterable: true },
                    { text: 'Email', value: 'email' , filterable: true },
                    { text: 'Country', value: 'country' , filterable: true },
                    { text: 'City', value: 'city' , filterable: true },
                    { text: 'Address', value: 'address' , filterable: true }
                ],
                users: [],
                paginationValues: [
                        { "name" : "Show 10" , "value" : 10 },
                        { "name" : "Show 20" , "value" : 20 },
                        { "name" : "Show 50" , "value" : 50 },
                        { "name" : "Show 100" , "value" : 100 },
                        { "name" : "Show 1000" , "value" : 1000 },
                        { "name" : "Show All" , "value" : 10000 },
                    ],
                affectedRows: ''
        } },
        mounted() {
            let self = this;
            axios.get("/metadata/metadata.json")
                .then((response) => {
                    //add unique ids
                    for(let i = 0 ; i < 10000; i++){
                        response.data[i].new_id = i+1;
                    }
                    response.data.forEach(function (val) {
                            self.users.push({
                                id: val.new_id,
                                first_name: val.first_name,
                                last_name: val.last_name,
                                user_name: val.user_name,
                                gender: val.gender,
                                age: val.age,
                                email: val.email,
                                country: val.country,
                                city: val.city,
                                address: val.address,
                            });
                    });
                })
                .catch((error) => {
                    console.log(`[ERROR] ${error}`);
                })
        },
    }
</script>
