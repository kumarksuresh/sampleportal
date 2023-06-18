<template>
  <v-app>
    <v-card>
      <v-layout>
        <v-navigation-drawer>
          <v-list>
            <v-list-item prepend-avatar="https://randomuser.me/api/portraits/men/88.jpg" title="John Doe"
              subtitle="johnDoe_88@gmailcom"></v-list-item>
          </v-list>

          <v-divider></v-divider>

          <v-list density="compact" nav>
            <v-list-item prepend-icon="mdi-folder" title="My Insureds" value="insureds" @click="clickInsured">
            </v-list-item>
            <v-list-item prepend-icon="mdi-file-sign" title="My Policies" value="policies" @click="clickPolcies">
            </v-list-item>
          </v-list>
        </v-navigation-drawer>

        <v-main>          
          <default-view></default-view>
        </v-main>
      </v-layout>
    </v-card>
  </v-app>
</template>

<script setup>
import { useRouter } from 'vue-router';
import DefaultView from './View.vue'

const router = useRouter();


function clickInsured() {  
  router.push({ name: 'Insured' })
}

function clickPolcies() {
  router.push({ name: 'Policies' })
}
</script>
