<script setup>
    definePageMeta({
        middleware: 'auth'
    })

    const user = useSupabaseUser()
</script>

<template>
    <div>
        Hi, My Name is: {{ user.email }}
    </div>
</template>