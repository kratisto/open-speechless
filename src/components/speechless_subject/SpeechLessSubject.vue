<template>
    <div>
        <div id="table">
            <v-expansion-panels>
                <v-expansion-panel
                        :key="data"
                >
                    <v-expansion-panel-header>Subject</v-expansion-panel-header>
                    <v-expansion-panel-content>
                        <v-data-table
                                :headers="columns"
                                :items="subjects"
                                :hide-default-footer="true"
                                class="elevation-1"
                        >
                            <template v-slot:top>
                                <v-toolbar flat color="white">

                                    <v-spacer></v-spacer>
                                    <v-dialog v-model="dialog" max-width="500px">
                                        <template v-slot:activator="{ on }">
                                            <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
                                        </template>
                                        <v-card>
                                            <v-card-title>
                                                <span class="headline">{{ formTitle }}</span>
                                            </v-card-title>

                                            <v-card-text>
                                                <v-container>
                                                    <v-row>
                                                        <v-col cols="12" sm="6" md="4">
                                                            <v-text-field v-model="editedItem.Nom" label="Nom "></v-text-field>
                                                        </v-col>
                                                        <v-col cols="12" sm="6" md="4">
                                                            <v-text-field v-model="editedItem.Lien" label="Lien"></v-text-field>
                                                        </v-col>
                                                    </v-row>
                                                </v-container>
                                            </v-card-text>

                                            <v-card-actions>
                                                <v-spacer></v-spacer>
                                                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                                                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                                            </v-card-actions>
                                        </v-card>
                                    </v-dialog>
                                </v-toolbar>
                            </template>
                            <template v-slot:item.action="{ item }">
                                <v-icon
                                        small
                                        class="mr-2"
                                        @click="editItem(item)"
                                >
                                    edit
                                </v-icon>
                                <v-icon
                                        small
                                        @click="deleteItem(item)"
                                >
                                    delete
                                </v-icon>
                            </template>
                        </v-data-table>
                    </v-expansion-panel-content>
                </v-expansion-panel>
            </v-expansion-panels>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SpeechlessSubject',
    data: function () {
        return {
            columns: [
                { text: 'Name', value: 'Nom' },
                { text: 'Actions', value: 'action', sortable: false },
            ],
            container: null,
            subjects : [
                {Nom:"Un film",  Lien:""},
                {Nom:"Une serie", Lien:""},
                {Nom:"Un logiciel", Lien:""}
            ],
            dialog: false,
            editedIndex: -1,
            editedItem: {
                Nom: "",
                Lien: "",
            },
            defaultItem: {
                Nom: "",
                Lien: "",
            },
        }
    },
    methods : {
        getDataSpin(){
            return this.subjects;
        },
        editItem (item) {
            this.editedIndex = this.subjects.indexOf(item)
            this.editedItem = Object.assign({}, item)
            this.dialog = true
        },
        deleteItem (item) {
            const index = this.subjects.indexOf(item)
            confirm('Are you sure you want to delete this item?') && this.subjects.splice(index, 1)

            this.$emit("subjects",this.subjects)
        },
        close () {
            this.dialog = false
            setTimeout(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            }, 300)
        },
        save () {
            if (this.editedIndex > -1) {
                Object.assign(this.subjects[this.editedIndex], this.editedItem)
            } else {
                this.subjects.push(this.editedItem)
            }
            this.close()
            this.$emit("subjects",this.subjects)
        },
    },
    watch: {
        dialog (val) {
            val || this.close()
        },
    },
}
</script>

<style>
    text{
        font-family:Helvetica, Arial, sans-serif;
        font-size:11px;
        pointer-events:none;
    }

    #table {
        padding: 20px;
    }
</style>
