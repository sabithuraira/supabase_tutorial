<script setup>
  // import { createClient } from '@supabase/supabase-js'
  // const supabase = createClient('your_url', 
  //   'your anon key')
  
  const supabase = useSupabaseClient()
  const user = useSupabaseUser()

  const datas = ref([])
  const form = ref({ id: "", name: "", desc: "", category_id: ""}) 
  const search_keyword = ref("")
  const categories =  ref([])

  //get data
  async function getDatas(keyword = "") {
    // const { data } = await supabase.from('animals')
    //     .select(`*, categories(id, name)`)
    let query = supabase.from('animals')
        .select(`*, categories(id, name)`)

    if(keyword!=""){
      // query = query.ilike('name', '%' + keyword +'%')
      query = query.or(
              'name.ilike.%' + keyword +'%,description.ilike.%' + keyword +'%',
            )
    }

    const { data } = await query
    datas.value = data
    getCategories()
  }
  
  //get list of categories
  async function getCategories() {
    const { data } = await supabase.from('categories').select()
    categories.value = data
  }

  function formUpdate(id, name, desc, category_id){
    form.value = { id: id, name: name, desc: desc, category_id: category_id}
  }

  async function saveData(id, name, desc, category_id){
    let form_object = { name: name, description: desc, category_id: category_id }

    if(id==""){
        const { data, error } = await supabase.from('animals')
            .insert(form_object)
            .select(`*, categories(id, name)`)
        datas.value.push(data[0])
    }
    else{
        const { error } = await supabase.from('animals')
            .update(form_object)
            .eq('id', id)
        getDatas()
    }

    form.value = { id: "", name: "", desc: ""}
  }

  async function deleteData(id){
    const response = await supabase.from('animals')
          .delete()
          .eq('id', id)

    getDatas()
  }

  const signOut = async () => {
    const { error } = await supabase.auth.signOut()

    if (error) console.log(error)
    else alert("Success Sign Out")
  }

  onMounted(() => {
    form.value = { id: "", name: "", desc: ""}
    getDatas()
  })
</script>

<template>
  <div>
    <div v-if="!user">
      <a href="/login">Login</a><br/>
      <a href="/signup">Signup</a><br/>
    </div>
    <div v-else>
      <a href="#" @click="signOut()">Sign Out</a><br/>
      <a href="/aboutme">About Me</a><br/>
    </div>
    
    <input type="text" v-model="search_keyword"/> 
    <button @click="getDatas(search_keyword)">Search</button>
    <ul>
      <li v-for="data in datas" :key="data.id">
        <a href="#" @click="formUpdate(data.id, data.name, data.description, data.category_id)">Edit</a> | 
        <a href="#" @click="deleteData(data.id)">Delete</a>  
        {{ data.name }} - {{ data.description }}  {{ data.category_id==null ? "" : " - " + data.categories.name }}
      </li>
    </ul>

    Name : <input type="text" v-model="form.name" /> <br/>
    Desc : <input type="text" v-model="form.desc" /> <br/>
    Categories : 
      <select v-model="form.category_id">
        <option v-for="c in categories" :key="c.id" :value="c.id" :selected="form.category_id==c.id ? 'selected' : ''">
          {{ c.name }}
        </option>
      </select><br/>
    <button @click="saveData(form.id, form.name , form.desc, form.category_id)">Save Data</button>
  </div>
</template>