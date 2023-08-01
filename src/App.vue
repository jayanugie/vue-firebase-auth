<template>
  <div>
    <nav>
      <RouterLink to="/">Home </RouterLink>|
      <RouterLink to="/feed"> Feed </RouterLink>|
      <RouterLink to="/register"> Register </RouterLink>|
      <RouterLink to="/sign-in"> Login </RouterLink>|
      <button @click="handleSignOut" v-if="isLoggedIn "> Sign out </button>
    
    </nav>
    <div class="col">
      <RouterView />
    </div>
  </div>
</template>

<script setup>
import { RouterLink, RouterView, useRouter } from 'vue-router'
import { onMounted, ref } from "vue"
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth';

const isLoggedIn = ref(false)
const router = useRouter()

let auth;
onMounted(() => {
  auth = getAuth()
  onAuthStateChanged(auth, (user) => {
    if (user) {
      isLoggedIn.value = true
    } else {
      isLoggedIn.value = false
    }
  })
})

const handleSignOut = () => {
  signOut(auth).then(() => {
    router.push("/")
  })
}

</script>


<style scoped>
.col {
  display: flex;
  flex-direction: column;
}

.center {
  display: flex;
  justify-content: center;
}
</style>