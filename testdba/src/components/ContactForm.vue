<script>
    import UseButton from "@/UI/UseButton.vue";
    export default {
        component: {
            UseButton
        },
        props: {
            receivedContact: {
                type: Object,
                default: () => ({
                    id: 0,
                    last_name: "",
                    first_name: "",
                    patronymic: "",
                    mobile_phone_number: null,
                    home_phone_number: null,
                    adress: ""
                })
            },
            buttonText: {
                type: String
            }
        },
        data() {
            return {
                new_contact: {
                    id: this.receivedContact.id,
                    last_name: this.receivedContact.last_name,
                    first_name: this.receivedContact.first_name,
                    patronymic: this.receivedContact.patronymic,
                    mobile_phone_number: this.receivedContact.mobile_phone_number,
                    home_phone_number: this.receivedContact.home_phone_number,
                    adress: this.receivedContact.adress,
                }
            }
        },
        methods: {
            createContact() {
                console.log("createContact_CF1")

                if (this.new_contact.id === 0) {
                    this.new_contact.id = Date.now()
                }

                this.$emit('func', this.new_contact)

                console.log("createContact_CF2")
            },
            exit() {
                console.log("exit")
                this.$emit('exit', false)
            },

            inputPhoneNumber(event) {
                console.log("inputPhoneNumber")

                const inp_str = event.target.value
                var str_num =""
                for (let i = 0; i < inp_str.length; i++) {
                    if (inp_str[i] >= '0' && inp_str[i] <= '9') {
                        str_num = str_num + inp_str[i]
                    }
                }
                this.new_contact.mobile_phone_number = str_num;
            },
            inputHomeNumber(event) {
                console.log("inputHomeNumber")

                const inp_str = event.target.value
                var str_num = ""
                for (let i = 0; i < inp_str.length; i++) {
                    if (inp_str[i] >= '0' && inp_str[i] <= '9') {
                        str_num = str_num + inp_str[i]
                    }
                }
                this.new_contact.home_phone_number = str_num;
            },
            inputFirstName(event) {
                const inp_str = event.target.value
                var str_right = ""
                for (let i = 0; i < inp_str.length; i++) {
                    if (inp_str[i].toLowerCase() != inp_str[i].toUpperCase()) {
                        str_right = str_right + inp_str[i]
                    }
                }
                this.new_contact.first_name = str_right;
            },
            inputLastName(event) {
                const inp_str = event.target.value
                var str_right = ""
                for (let i = 0; i < inp_str.length; i++) {
                    if (inp_str[i].toLowerCase() != inp_str[i].toUpperCase()) {
                        str_right = str_right + inp_str[i]
                    }
                }
                this.new_contact.last_name = str_right;
            },
            inputPatronymic(event) {
                const inp_str = event.target.value
                var str_right = ""
                for (let i = 0; i < inp_str.length; i++) {
                    if (inp_str[i].toLowerCase() != inp_str[i].toUpperCase()) {
                        str_right = str_right + inp_str[i]
                    }
                }
                this.new_contact.patronymic = str_right;
            },
            inputAdress(event) {
                const inp_str = event.target.value
                var str_right = ""
                for (let i = 0; i < inp_str.length; i++) {
                    if (inp_str[i].toLowerCase() != inp_str[i].toUpperCase() || inp_str[i] >= '0' && inp_str[i] <= '9' || inp_str[i] == '.' || inp_str[i] == ',' || inp_str[i] == '-' || inp_str[i] == '/') {
                        str_right = str_right + inp_str[i]
                    }
                }
                this.new_contact.adress = str_right;
            },
        }
    }
</script>

<template>
    <form @submit.prevent>
        <h4>Создание контакта</h4>
        <input v-bind:value="new_contact.last_name"
               @input="inputLastName"
               class="input"
               placeholder="Фамилия" />

        <input v-bind:value="new_contact.first_name"
               @input="inputFirstName"
               class="input"
               placeholder="Имя" />

        <input v-model="new_contact.patronymic"
               @input="inputPatronymic"
               class="input"
               placeholder="Отчество" />

        <input v-bind:value="new_contact.mobile_phone_number"
               @input="inputPhoneNumber"
               class="input"
               placeholder="Номер мобильного телефона" />

        <input v-bind:value="new_contact.home_phone_number"
               @input="inputHomeNumber"
               class="input"
               placeholder="Номер домашнего телефона" />

        <input v-bind:value="new_contact.adress"
               @input="inputAdress"
               class="input"
               placeholder="Адрес" />
        <div style="display:flex">
            <use-button class="btn"
                        style=" margin-right: 15px; margin-top: 15px "
                        @click="exit">
                Отмена
            </use-button>

            <use-button class="btn"
                        style="margin-right: 15px; margin-top: 15px"
                        @click="createContact">
                {{buttonText}}
            </use-button>
        </div>
    </form>  
</template>

<style>
form {
    display: flex;
    flex-direction: column;
}
.input {
    width: 100%;
    border: 1px solid teal;
    padding: 10px 15px;
    margin-top: 15px;
}
</style>