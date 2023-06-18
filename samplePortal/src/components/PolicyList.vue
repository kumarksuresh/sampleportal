<template>
    <v-app-bar flat>
    <template v-slot:prepend>
      <v-icon icon="mdi-apps" />
    </template>
    <v-app-bar-title> Policy List</v-app-bar-title>
  </v-app-bar>
    <v-table style="width: 90%;">
        <thead>
        <tr>
            <th></th>
            <th class = "text-left">Insured Name</th>
            <th class = "text-left">Policy Number</th>
            <th class = "text-left">Type</th>
            <th class = "text-left">Start Date</th>
            <th class = "text-left">End Date</th>
            <th class = "text-center">Premium</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="item in policies" :key="item.policyNumber">
            <td><v-img :src="item.image" contain style="width: 70%; height: 70%;"></v-img></td>
            <td class="text-left">{{item.insured.lastName}} {{ item.insured.firstName }}</td>
            <td class="text-left">{{ item.policyNumber }}</td>
            <td class="text-left"><v-icon>{{ getpolicyTypeIcon(item.policyType.typeName) }}</v-icon></td>
            <td class="text-left">{{ format(parseISO(item.policyStartDate), 'yyyy-MM-dd') }}</td>
            <td class="text-left">{{ format(parseISO(item.policyEndDate), 'yyyy-MM-dd') }}</td>
            <td class="text-center">$ {{ item.premiumAmount }}</td>
        </tr>
    </tbody>
    </v-table>
</template>
<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { format } from 'date-fns'
import { parseISO } from 'date-fns'


const policies = ref(null);
async function fetchPolicies() {
    const response = await axios.get('https://localhost:7081/api/Insured/GetPolcies')
    policies.value = response.data

    policies.value.forEach(element => {        
        var gender = element.insured.gender == 'M' ? "men" : "women"        
        element.image = 'https://randomuser.me/api/portraits/'+ gender +'/'+ element.insured.id + '.jpg'

        
    });
}

function getpolicyTypeIcon(policyType){
    if(policyType === 'Life Insurance')
    return 'mdi-human-male-female-child'
    else if (policyType === 'Health Insurance')
    return 'mdi-head-plus'
    else if (policyType === 'Auto Insurance')
    return 'mdi-car-clock'
    else if (policyType === 'Home Insurance')
    return 'mdi-home-circle'
    else if (policyType === 'Travel Insurance')
    return 'mdi-wallet-travel'
}

fetchPolicies()
</script>