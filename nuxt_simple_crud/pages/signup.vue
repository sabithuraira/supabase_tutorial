
<script setup>
  const supabase = useSupabaseClient()

  const form = ref({ email: "", password: ""})

  const signIn = async () => {

    const { data, error } = await supabase.auth.signUp({
      email: form.email,
      password: form.password,
    })

    if (error) console.log(error)
    else console.log('User signed up successfully:', data)
  }
</script>

<template>
  <div>

    Email : <input type="text" v-model="form.email" /> <br/>
    Password : <input type="text" v-model="form.password" /> <br/>
    
    <button @click="signIn()">Sign In</button>
  </div>
</template>