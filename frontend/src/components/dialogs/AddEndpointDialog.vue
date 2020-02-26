<template>
    <v-row justify="center">
        <v-dialog v-model="show" persistent max-width="600px">
            <v-card>
                <v-card-title>
                    <span class="headline">Add endpoint</span>
                </v-card-title>
                <v-card-text>
                    <v-container>
                        <v-row>
                            <v-col cols="8" md="8">
                                <v-text-field v-model="name" label="Endpoint name*" required></v-text-field>
                            </v-col>
                            <v-col cols="12" md="12">
                                <v-text-field v-model="description" label="Description"></v-text-field>
                            </v-col>
                            <v-col cols="12" sm="6" md="6">
                                <v-text-field v-model="address" label="Address*" hint="http(s): or IP address" required></v-text-field>
                            </v-col>
                            <v-col cols="12" sm="6">
                                <v-select
                                        :items="['5 min', '10 min', '15 min', '30 min', '1 hour']" v-model="updateInterval" label="Update interval*"
                                        required
                                ></v-select>
                            </v-col>
                        </v-row>
                    </v-container>
                    <small>*indicates required field</small>
                </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="show = false">Close</v-btn>
                    <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-row>
</template>

<script>
    export default {
        data() {
           return {
               name: undefined,
               description: undefined,
               address: undefined,
               updateInterval: undefined
           }
        }, methods: {
            save() {
                this.$emit('onEndpointAdded', this.name, this.description, this.address, this.updateInterval)
                this.show = false;
            }
        }, props: {
            value: Boolean
        },
        computed: {
            show: {
                get () {
                    return this.value
                },
                set (value) {
                    this.$emit('input', value)
                }
            }
        }
    }
</script>

<style scoped>

</style>