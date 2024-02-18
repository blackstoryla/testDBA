<template>
    <div class="app">
        <h1>Телефонный справочник</h1>
        <div style="margin-top:30px; margin-bottom:30px;">
            <use-button class="btn"
                        @click="showDialog">Создать новый контакт</use-button>
            <use-button class="btn"
                        @click="generateCSV"
                        style="margin-left: 15px;">Экспортировать CSV</use-button>
            <!--<div style="margin-top:30px; margin-bottom:30px;">
                <input type="file" @change="handleFileInput" />
            </div>-->
        </div>

        <use-dialog v-model:show="dialogVisibleCreate">
            <contact-form buttonText="Создать"
                          @func="createContact"
                          @exit="closeDialog"/>
        </use-dialog>

        <use-dialog v-model:show="dialogVisibleUpdate">
            <contact-form :receivedContact="contactToUpdate"
                          buttonText="Изменить"
                          @func="updateContact" 
                          @exit="closeDialog"/>
        </use-dialog>

        <contact-list v-bind:contact_list="contact_list"
                      @remove="removeContact"
                      @update="updateContactShowDialog" />
    </div>
</template>


<script>
    import ContactList from "@/components/ContactList.vue";
    import ContactForm from "@/components/ContactForm.vue";
    import UseDialog from "@/UI/UseDialog.vue";
    /*import { readAsText } from "file-reader";*/

    export default {
        components: {
            ContactList, ContactForm, UseDialog
        },

        data() {
            return {
                contact_list: [
                    { id: 1708258549472, last_name: "Иванов", first_name: "Петр", patronymic: "Сергеевич", home_phone_number: 111222, mobile_phone_number: 79123456789, adress: "Барнаул" },
                    { id: 1708258516463, last_name: "Кузнецов", first_name: "Виталий", patronymic: "Вениаминович", home_phone_number: 333000, mobile_phone_number: 79123456789, adress: "Новосибирск" },
                    { id: 1708258871456, last_name: "Алексеев", first_name: "Сергей", patronymic: "Михайлович", home_phone_number: 333333, mobile_phone_number: 79123456789, adress: "Омск" },
                    { id: 1708258590268, last_name: "Леонтьева", first_name: "Мария", patronymic: "Александровна", home_phone_number: 555555, mobile_phone_number: 79123456789, adress: "Кемерово" },
                ],
                dialogVisibleCreate: false,
                dialogVisibleUpdate: false,
                contactToUpdate: null
            }
        },
        methods: {
            createContact(new_contact) {
                console.log("createContact")

                if (new_contact.first_name.length < 1 && new_contact.last_name.length < 1) {
                    alert("Введите фамилию и имя")
                    return
                }

                if (String(new_contact.mobile_phone_number).length < 11) {
                    alert("Введите номер мобильного телефона  " + new_contact.mobile_phone_number)
                    return
                }

                if (String(new_contact.home_phone_number).length < 6) {
                    alert("Введите номер домашнего телефона")
                    return
                }

                if (new_contact.adress.length < 1) {
                    alert("Введите адрес")
                    return
                }

                for (let i = 0; i < this.contact_list.length; i++) {
                    if (new_contact.first_name == this.contact_list[i].first_name && new_contact.last_name == this.contact_list[i].last_name && new_contact.patronymic == this.contact_list[i].patronymic) {
                        this.dialogVisibleCreate = false
                        alert("Контакт с таким ФИО уже существует")
                        return
                    }
                }

                this.contact_list.push(new_contact)
                this.dialogVisibleCreate = false
            },
            removeContact(contact) {
                console.log("removeContact")

                var result = confirm("Вы действительно хотите удалить контакт?");
                if (!result) {
                    return
                }

                this.contact_list = this.contact_list.filter(c => c.id != contact.id)
            },
            updateContact(contact) {
                console.log("updateContact")

                var result = confirm("Вы действительно хотите изменить контакт?");
                if (!result) {
                    return
                }


                for (let i = 0; i < this.contact_list.length; i++) {
                    if (this.contact_list[i].id == contact.id) {
                        this.contact_list[i] = contact
                    }
                }
                this.dialogVisibleUpdate = false
            },
            showDialog(contact) {
                console.log("showDialog")
                this.dialogVisibleCreate = true
            },
            closeDialog() {
                console.log("closeDialog")
                this.dialogVisibleCreate = false
                this.dialogVisibleUpdate = false
            },
            updateContactShowDialog(contact) {
                console.log("updateContactShowDialog")
                this.dialogVisibleUpdate = true
                this.contactToUpdate = contact
            },
            generateCSV() {
                let csvContent = 'data:text/csv;charset=utf-8,';
                var csvData = []

                for (let i = 0; i < this.contact_list.length; i++) {
                    csvData = [this.contact_list[i].id, this.contact_list[i].last_name, this.contact_list[i].first_name, this.contact_list[i].patronymic, this.contact_list[i].home_phone_number, this.contact_list[i].mobile_phone_number, this.contact_list[i].adress]
                    const csvRow = csvData.join(';');
                    csvContent += csvRow + '\r\n';
                }

                const encodedUri = encodeURI(csvContent);
                const link = document.createElement('a');
                link.setAttribute('href', encodedUri);
                link.setAttribute('download', 'data.csv');
                document.body.appendChild(link);

                link.click();

                document.body.removeChild(link);
            },
            //handleFileInput(event) {
            //    var data_from_csv = []
            //
            //    const file = event.target.files[0];
            //    if (file.type === "text/csv") {
            //        readAsText(file).then((text) => {   //Не работает readAsText
            //            const rows = text.split("\n");
            //            for (let i = 0; i < rows.length; i++){
            //                data_from_csv.push(rows[i].split(";"))
            //            }
            //        });
            //    }
            //
            //    for (let i = 0; i < data_from_csv.length; i++){
            //          const new_contact = {
            //              id: data_from_csv[i][0],
            //              last_name: data_from_csv[i][1],
            //              first_name: data_from_csv[i][2],
            //              patronymic: data_from_csv[i][3],
            //              home_phone_number: data_from_csv[i][4],
            //              mobile_phone_number: data_from_csv[i][5],
            //              adress: data_from_csv[i][6]
            //          }
            //          this.contact_list.push(new_contact)
            //    }
            //}
        }
    }
</script>


<style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box
        }

        .app {
            padding: 20px;
        }
</style>
