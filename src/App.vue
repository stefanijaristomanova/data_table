<template>
    <v-app>
        <v-content>
            <users-table></users-table>
        </v-content>
        <Footer></Footer>
    </v-app>
</template>

<script>
     import Footer from "./components/footer.vue";
    import UsersTable from "./components/usersTable.vue";

    export default {
        name: 'App',
        components: {
            UsersTable,
            Footer,
        },
    };
</script>
