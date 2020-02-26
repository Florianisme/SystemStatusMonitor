<template>
    <v-app id="inspire">
        <v-navigation-drawer v-model="drawer" app clipped>
            <v-list dense>
                <v-list-item link>
                    <v-list-item-action>
                        <v-icon>mdi-view-dashboard</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title>Dashboard</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
                <v-list-item link>
                    <v-list-item-action>
                        <v-icon>mdi-settings</v-icon>
                    </v-list-item-action>
                    <v-list-item-content>
                        <v-list-item-title>Settings</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <v-app-bar
                app
                clipped-left>
            <v-app-bar-nav-icon @click.stop="drawer = !drawer"/>
            <v-toolbar-title>System Status</v-toolbar-title>
        </v-app-bar>

        <v-content>
            <h2 class="ma-6">System Overview</h2>
            <v-data-table class="ma-4" :headers="headers" :items="dummySystems" hide-default-footer>
                <template v-slot:item.status="{ item }">
                    <v-chip :color="getColorFromStatus(item.status)" dark>{{ item.status }}</v-chip>
                </template>
            </v-data-table>
            <v-btn bottom right dark fixed fab color="blue" @click.stop="showAddEndpointDialog=true">
                <v-icon>mdi-plus</v-icon>
            </v-btn>
            <AddEndpointDialog v-model="showAddEndpointDialog" v-on:onEndpointAdded="onEndpointAdded"/>
        </v-content>
    </v-app>
</template>

<script>
    import AddEndpointDialog from "@/components/dialogs/AddEndpointDialog";

    export default {
        components: {AddEndpointDialog},
        data() {
            return {
                drawer: null,
                showAddEndpointDialog: false,
                dummySystems: [{
                    name: "Prod Environment",
                    description: "Used for receiving and sending partner messages",
                    status: "Online",
                    lastChecked: "10 Minutes ago",
                    uptimePercentage: "98,39%"
                }, {
                    name: "Test Environment",
                    description: "Playground for new partner",
                    status: "Offline",
                    lastChecked: "10 Minutes ago",
                    uptimePercentage: "67,70%"
                }],
                headers: [
                    {text: 'Name', value: 'name'},
                    {text: 'Description', value: 'description'},
                    {text: 'Status', value: 'status'},
                    {text: 'Last Update', value: 'lastChecked'},
                    {text: 'Uptime (%)', value: 'uptimePercentage'},
                ]
            }
        }, methods: {
            getColorFromStatus(status) {
                if (status === 'Online') {
                    return 'green'
                } else if (status === 'Offline'){
                    return 'red'
                } else {
                    return 'dark-gray'
                }
                // eslint-disable-next-line no-unused-vars
            }, onEndpointAdded(name, description, address, updateInterval) {
                // TODO save to DB
                this.dummySystems.push({name: name, description: description, status: 'Unknown', lastChecked: 'never', uptimePercentage: ''})
            }
        }, created() {
            this.$vuetify.theme.dark = false
        }
    }
</script>