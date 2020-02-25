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
        </v-content>
    </v-app>
</template>

<script>
    export default {
        data() {
            return {
                drawer: null,
                dummySystems: [{
                    name: "Prod Environment",
                    description: "Used for receiving and sending partner messages",
                    status: "Running",
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
                return status === 'Running' ? 'green' : 'red'
            }
        }, created() {
            this.$vuetify.theme.dark = true
        }
    }
</script>