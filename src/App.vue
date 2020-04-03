<template>
    <v-app>
        <v-content>
            <v-container class="fill-height"
                         fluid>
                <v-row align="center"
                       justify="center">
                    <v-col cols="12"
                           sm="8"
                           md="4"
                           v-if="!isLogged">
                        <v-card class="elevation-12">
                            <v-toolbar color="primary"
                                       dark
                                       flat>
                                <v-toolbar-title>Login form</v-toolbar-title>
                            </v-toolbar>
                            <v-card-text>
                                <v-form>
                                    <v-text-field label="Login"
                                                  prepend-icon="mdi-account"
                                                  type="text"
                                                  v-model="login"/>
                                    <v-text-field label="Password"
                                                  prepend-icon="mdi-lock"
                                                  type="password"
                                                  v-model="password"/>
                                </v-form>
                            </v-card-text>
                            <v-card-actions>
                                <v-spacer/>
                                <v-btn color="primary" @click="doLogin()">Login</v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-col>
                    <v-col cols="12"
                           sm="8"
                           md="4"
                           v-else>
                        <v-card class="elevation-12">
                            <v-toolbar color="primary"
                                       dark
                                       flat>
                                <v-toolbar-title>Contacts List</v-toolbar-title>
                            </v-toolbar>
                            <v-card-text>

                                <h3>Contact</h3>
                                <v-form>
                                    <v-text-field label="Name"
                                                  prepend-icon="mdi-account"
                                                  type="text"
                                                  v-model="contactData.name"/>
                                    <v-text-field label="Phone"
                                                  prepend-icon="mdi-phone"
                                                  v-model="contactData.phone"/>
                                </v-form>
                                <v-btn color="primary" @click="saveContact()">Save Contact</v-btn>
                                <v-divider class="mt-4 mb-4"></v-divider>

                                <v-simple-table>
                                    <template v-slot:default>
                                        <thead>
                                        <tr>
                                            <th class="text-left">Name</th>
                                            <th class="text-left">Phone</th>
                                            <th class="text-center">Actions</th>
                                        </tr>
                                        </thead>
                                        <tbody>
                                        <tr v-for="contact in contacts" :key="contact.phone">
                                            <td>{{ contact.name }}</td>
                                            <td>{{ contact.phone }}</td>
                                            <td class="text-center">
                                                <v-btn class="mx-2" fab dark small color="primary"
                                                       @click="editContact(contact)">
                                                    <v-icon dark>mdi-pencil</v-icon>
                                                </v-btn>
                                                <v-btn class="mx-2" fab dark small color="error"
                                                       @click="deleteContact(contact)">
                                                    <v-icon dark>mdi-delete</v-icon>
                                                </v-btn>
                                            </td>
                                        </tr>
                                        </tbody>
                                    </template>
                                </v-simple-table>

                            </v-card-text>
                        </v-card>
                    </v-col>
                </v-row>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>
  export default {
    name: 'App',
    data: () => ({
      isLogged: false,
      login: '',
      password: '',
      contacts: [],
      contactData: {
        name: '',
        phone: ''
      }
    }),
    methods: {
      doLogin () {
        this.isLogged = true
      },
      editContact (contact) {
        this.contactData = { ...contact }
      },
      deleteContact (contact) {
        this.contacts = this.contacts.filter(item => item.phone !== contact.phone)
      },
      saveContact () {
        // Delete if contact exists
        this.deleteContact(this.contactData)
        // Add the new contact to list
        this.contacts.push({ ...this.contactData })
        this.contactData.name = ''
        this.contactData.phone = ''
      }
    }
  }
</script>
