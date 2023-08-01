<template>
    <h1>Create an Account</h1>
    <p><input type="text" placeholder="Email" v-model="email"></p>
    <p><input type="text" placeholder="Password" v-model="password"></p>
    <p><button @click="register">Submit</button></p>
    <p><button @click="signInWithGoogle">Sign In with Google</button></p>
</template>

<script setup>
import { ref } from "vue"
import { 
    getAuth, 
    createUserWithEmailAndPassword,
    GoogleAuthProvider,
    signInWithPopup
 } from "firebase/auth";
import { useRouter } from 'vue-router'

const email = ref("")
const password = ref("")
const router = useRouter()


const register = () => {
    createUserWithEmailAndPassword(getAuth(), email.value, password.value)
    .then((data) => {
        console.log("Successfully registered!")
        router.push('/feed')
    })
    .catch((err) => {
        console.log(err.code)
        alert(err.message )
    })
}

const signInWithGoogle = () => {
    const provider = new GoogleAuthProvider()
    signInWithPopup(getAuth(), provider)
        .then((res) => {
            console.log(res.user)
            router.push("/feed")
        })
        .catch ((err) => {
            switch (err.code) {
            case "auth/invalid-email":
                errMsg.value = "Invalid email"
                break
            case "auth/user-not-found":
                errMsg.value = "No account with that email was found"
                break
            case "auth/wrong-password":
                errMsg.value = "Incorrect password"
                break
            default:
                errMsg.value = "Email or password was incorrect"
                break
            }    
        })
}

</script>

<style lang="scss" scoped>

</style>