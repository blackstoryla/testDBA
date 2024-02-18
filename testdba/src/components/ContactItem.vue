<script>
    import UseButton from "@/UI/UseButton.vue";
    export default {
        component: {
            UseButton
        },
        props: {
            contact: {
                type: Object,
                required: true
            }
        }
    }
</script>

<template>
    <div class="contact">
        <div>
            <div>
                <strong>Фамилия: </strong>{{contact.last_name}}
                <strong>Имя: </strong>{{contact.first_name}}
                <strong>Отчество: </strong>{{contact.patronymic}}
            </div>
            <div>
                <strong>Номер мобильного телефона: </strong>{{contact. mobile_phone_number}}
            </div>
            <div>
                <strong>Номер домашнего телефона: </strong>{{contact. home_phone_number}}
            </div>
            <div>
                <strong>Адрес: </strong>{{contact.adress}}
            </div>
        </div>
        <div style="display:flex">
            <use-button class="btn"
                        style="margin-right: 15px"
                        @click="$emit('update', contact)">
                Обновить
            </use-button>
            <use-button class="btn"
                        @click="$emit('remove', contact)">
                Удалить
            </use-button>
        </div>

    </div>
</template>
 
<style scoped>
    .contact {
        padding: 15px;
        border: 2px solid teal;
        margin-top: 15px;
        display:flex;
        align-items: center;
        justify-content: space-between;
    }
</style>