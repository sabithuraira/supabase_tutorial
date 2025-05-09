
<script setup>
  const supabase = useSupabaseClient()

  const form = ref({ email: "", password: ""})

  const signIn = async () => {
    // let form_object = { name: name, description: desc, category_id: category_id }

    // if(id==""){
    //     const { data, error } = await supabase.from('animals')
    //         .insert(form_object)
    //         .select(`*, categories(id, name)`)
    //     datas.value.push(data[0])
    // }
    // else{
    //     const { error } = await supabase.from('animals')
    //         .update(form_object)
    //         .eq('id', id)
    //     getDatas()
    // }

    // form.value = { id: "", name: "", desc: ""}

    const { data, error } = await supabase.auth.signInWithPassword({
      email: form.email,
      password: form.password,
    })

    if (error) console.log(error)
  }

// const signInWithOAuth = async () => {
//   const { error } = await supabase.auth.signInWithOAuth({
//     provider: 'github',
//     options: {
//       redirectTo: 'http://localhost:3000/confirm',
//     },
//   })
//   if (error) console.log(error)
// }
</script>

<template>
  <div>

    Email : <input type="text" v-model="form.name" /> <br/>
    Password : <input type="text" v-model="form.desc" /> <br/>
    
    <button @click="signIn()">Sign In</button>
  </div>
</template>