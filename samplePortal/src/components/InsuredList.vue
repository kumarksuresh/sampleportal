<template>
    <v-app-bar flat>
    <template v-slot:prepend>
      <v-icon icon="mdi-apps" />
    </template>
    <v-app-bar-title> Insured List</v-app-bar-title>
  </v-app-bar>
    <v-container align="start" fluid>
        <v-row dense>
            <v-col v-for="item in insureds" :key="item" cols="3">
                <v-card>
                    <v-card-title>
                        <v-row align="center" dense>
                            <v-col>
                                <v-img :src="item.image" contain style="width: 100px; height: 100px;">
                                    <template v-slot:placeholder>
                                        <v-row class="fill-height ma-0" align="center" justify="center">
                                            <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                                        </v-row>
                                    </template>
                                </v-img>
                            </v-col>
                            <v-col>
                                <small>{{ item.lastName }} {{ item.firstName }}</small>
                                <v-spacer />
                                <small>{{ item.formatteddob }}</small>
                            </v-col>
                        </v-row>
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>
<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { format } from 'date-fns'
import { parseISO } from 'date-fns'
const insureds = ref(null)

async function fetchInsured() {

    const response = await axios.get('https://localhost:7081/api/Insured/GetInsuredList')    
    insureds.value = response.data
    insureds.value.forEach(element => {        
        var gender = element.gender == 'M' ? "men" : "women"        
        element.image = 'https://randomuser.me/api/portraits/'+ gender +'/'+ element.id + '.jpg',
        element.formatteddob = format(parseISO(element.dateOfBirth), 'yyyy-MM-dd')
    });
}

fetchInsured()
</script>