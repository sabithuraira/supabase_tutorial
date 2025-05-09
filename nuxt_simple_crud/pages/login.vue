
<script setup>
  const supabase = useSupabaseClient()

  const form = ref({ email: "", password: ""})

  const signIn = async () => {
    const { data, error } = await supabase.auth.signInWithPassword({
      email: form.value.email,
      password: form.value.password,
    })

    if (error) console.log(error)
    else{
      alert("Success Sign In")
      await navigateTo('/')
    }
  }

</script>

<template>
  <div>

    Email : <input type="text" v-model="form.email" /> <br/>
    Password : <input type="text" v-model="form.password" /> <br/>
    
    <button @click="signIn()">Sign In</button>
  </div>
</template>