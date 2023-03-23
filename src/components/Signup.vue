<template>
  <div class="max-w-sm m-auto my-8">
    <div class="border p-10 border-grey-light shadow rounded">
      <h3 class="text-2xl mb-6 text-grey-darkest"> Sign In Bro!</h3>
    <form @submit.prevent="signin">
      <div class="text-red" v-if="error"> {{ error }}</div>

      <div class="mb-6">
        <label for="email" class="label bg-red-600">E-mail address</label>
        <input type="email" v-model="email" class="input" id="email" placeholder="blabla@gmail..com">
      </div>

      <div class="mb-6">
        <label for="password" class="label">password</label>
        <input type="password" v-model="password" class="input" id="password" placeholder="bla1b2.z">
      </div>
      <div class="mb-6">
        <label for="password" class="label">password confirmation</label>
        <input type="password" v-model="password_confirmation" class="input" id="password_confirmation" placeholder="bla1b2.z">
      </div>

      <button type="submit" class="font-sans font-bold px-4 rounded cursos-point no-underline bg-green hover:bg-green-dark block w-full py-4 text-white items-center justifiy-center">Create Account</button>
      <div class="my-4"><router-link to="/" class="link font-sans font-bold ">Sign-in if you have a account</router-link></div>
    </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Signup',
  data () {
    return {
      email: '',
      password: '',
      password_confirmation: '',
      error: ''
    }
  },
  created () {
    this.checkSignedIn()
  },
  updated () {
    this.checkSignedIn()
  },
  methods: {
    signup () {
      this.$http.plain.post('/signup', { email: this.email, password: this.password, password_confirmation: this.password_confirmation })
        .then(response => this.signinSuccesful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccesfull (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/records')
    },
    signinFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || 'something went wrong'
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkSignedIn () {
      if (localStorage.signedIn) {
        this.$router.replace('/records')
      }
    }
  }
}
</script>
