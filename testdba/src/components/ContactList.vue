<script>
    import ContactItem from "@/components/ContactItem.vue";
    export default {
        components: {
            ContactItem
        },
        props: {
            contact_list: {
                type: Array,
                required: true
            }
        }
    }
</script>

<template>
    <div v-if="contact_list.length > 0">
        <h3>Список контактов</h3>
        <contact-item v-for="contact in contact_list"
                      :contact="contact"
                      :key="contact.id"
                      @remove="$emit('remove',contact)"
                      @update="$emit('update',contact)"/>
    </div>
    <div v-else>
        <h3>Нет контактов</h3>
    </div>
    
</template>

<style scoped>
</style>