<script setup>
    import { ref, onMounted } from 'vue'
    import { supabase } from './lib/supabaseClient'

    const datas = ref([])
    const form = ref({ id: "", name: "", desc: ""})

    //get data
    async function getDatas() {
        const { data } = await supabase.from('animals').select()
        datas.value = data
    }

    function formUpdate(id, name, desc){
        form.value = { id: id, name: name, desc: desc}
    }

    async function saveData(id, name, desc){
        if(id==""){
            const { data, error } = await supabase.from('animals')
                .insert({ name: name, description: desc })
                .select()
            datas.value.push(data[0])
        }
        else{
            const { error } = await supabase.from('animals')
                .update({ name: name, description: desc })
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

    onMounted(() => {
        form.value = { id: "", name: "", desc: ""}
        getDatas()
    })
</script>


<template>
    <div>
        <ul>
            <li v-for="data in datas" :key="data.id">
                <a href="#" @click="formUpdate(data.id, data.name, data.description)">Edit</a> |
                <a href="#" @click="deleteData(data.id)">Delete</a>
                {{ data.name }} - {{ data.description }}
            </li>
        </ul>

        Name : <input type="text" v-model="form.name" /> <br/>
        Desc : <input type="text" v-model="form.desc" /> <br/>
        <button @click="saveData(form.id, form.name , form.desc)">Save Data</button>
    </div>
</template>