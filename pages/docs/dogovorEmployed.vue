<template>
    <section class="section">
        <b-alert
            v-if="variant === 'success'"
            :show="dismissCountDown"
            fade
            variant="success"
            dismissible
            @dismissed="dismissCountDown = 0"
            @dismiss-count-down="countDownChanged"
            class="fixed-top w-100"
        >
            <p>Спасибо за заявку, в ближайшее время мы с вами свяжемся</p>
            <b-progress
                variant="success"
                :max="dismissSecs"
                :value="dismissCountDown"
                height="4px"
            ></b-progress>
        </b-alert>
        <b-alert
            v-if="variant === 'danger'"
            :show="dismissCountDown"
            fade
            variant="danger"
            dismissible
            @dismissed="dismissCountDown = 0"
            @dismiss-count-down="countDownChanged"
            class="fixed-top w-100"
        >
            <p>Что-то пошло не так</p>
            <b-progress
                variant="success"
                :max="dismissSecs"
                :value="dismissCountDown"
                height="4px"
            ></b-progress>
        </b-alert>
        <h2>Анкета заключения договора для cамозанятыx</h2>

        <b-form class="mt-4" @submit.prevent="submitHandler">
            <b-form-group
                label="ИНН:"
                label-for="input-1"
            >
                <b-form-input
                    id="input-1"
                    type="text"
                    placeholder="Введите номер"
                    v-model="$v.form.inn.$model"
                    :state="$v.form.inn.$dirty ? !$v.form.inn.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.inn.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.inn.numeric && $v.form.inn.required">Должны быть только цифры</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.inn.minLength && $v.form.inn.required && $v.form.inn.numeric">Это поле не должно быть меньше 10 цифр</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="ФИО:"
                label-for="input-2"
            >
                <b-form-input
                    id="input-2"
                    type="text"
                    placeholder="Введите ФИО"
                    v-model="$v.form.fio.$model"
                    :state="$v.form.fio.$dirty ? !$v.form.fio.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.fio.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Дата рождения:"
                label-for="input-3"
            >
                <b-form-datepicker
                    id="input-3"
                    placeholder="Выберите дату"
                    v-model="$v.form.myData.$model"
                    :state="$v.form.myData.$dirty ? !$v.form.myData.$invalid : null"
                ></b-form-datepicker>
                <b-form-invalid-feedback v-if="!$v.form.myData.required">Пожалуйста заполните дату</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Телефон:"
                label-for="input-4"
            >
                <b-form-input
                    id="input-4"
                    type="text"
                    placeholder="Введите телефон"
                    v-model="$v.form.phone.$model"
                    :state="$v.form.phone.$dirty ? !$v.form.phone.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.phone.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.phone.numeric && $v.form.phone.required">Должны быть только цифры</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.phone.minLength && $v.form.phone.required && $v.form.phone.numeric">Это поле не должно быть меньше 9 цифр</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Email:"
                label-for="input-5"
            >
                <b-form-input
                    id="input-5"
                    type="text"
                    placeholder="Введите email"
                    v-model="$v.form.email.$model"
                    :state="$v.form.email.$dirty ? !$v.form.email.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.email.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.email.email && $v.form.email.required">Не корректный email</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Серия и номер паспорта:"
                label-for="input-6"
            >
                <b-form-input
                    id="input-6"
                    type="text"
                    placeholder="Введите серию и номер паспорта"
                    v-model="$v.form.pasport.$model"
                    :state="$v.form.pasport.$dirty ? !$v.form.pasport.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.pasport.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.pasport.numeric && $v.form.pasport.required">Должны быть только цифры</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Дата выдачи паспорта:"
                label-for="input-7"
            >
                <b-form-datepicker
                    id="input-7"
                    placeholder="Выберите дату"
                    v-model="$v.form.pasdata.$model"
                    :state="$v.form.pasdata.$dirty ? !$v.form.pasdata.$invalid : null"
                ></b-form-datepicker>
                <b-form-invalid-feedback v-if="!$v.form.pasdata.required">Пожалуйста заполните дату</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Кем выдан паспорт:"
                label-for="input-8"
            >
                <b-form-input
                    id="input-8"
                    type="text"
                    v-model="$v.form.fms.$model"
                    :state="$v.form.fms.$dirty ? !$v.form.fms.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.fms.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Адрес регистрации:"
                label-for="input-9"
            >
                <b-form-input
                    id="input-9"
                    type="text"
                    placeholder="Укажите индекс, город, улицу, дом"
                    v-model="$v.form.adresRegistr.$model"
                    :state="$v.form.adresRegistr.$dirty ? !$v.form.adresRegistr.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.adresRegistr.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                label="Сайт (не обязательно)"
                label-for="input-10"
            >
                <b-form-input
                    id="input-10"
                    type="text"
                    placeholder="Введите ссылку на ваш сайт"
                    v-model="form.site"
                ></b-form-input>
            </b-form-group>

            <b-form-group
                label="Комментарий (не обязательно)"
                label-for="input-11"
            >
                <b-form-textarea
                    id="input-11"
                    placeholder="Введите текст..."
                    rows="6"
                    v-model.trim="form.dopinfo"
                ></b-form-textarea>
            </b-form-group>
            <b-button type="submit" class="mt-4" block variant="primary">Отправить</b-button>
        </b-form>
    </section>
</template>
<script>
    import { required, minLength, numeric, email } from 'vuelidate/lib/validators'
    export default {
        name: 'dogovorEmployed',
        data () {
            return {
                dismissSecs: 5,
                dismissCountDown: 0,
                variant: '',
                form: {
                    inn: null,
                    fio: null,
                    myData: null,
                    phone: null,
                    email: null,
                    pasport: null,
                    pasdata: null,
                    fms: null,
                    adresRegistr: null,
                    site: null,
                    dopinfo: null,
                }
            }
        },
        validations: {
            form: {
                inn: {
                    required,
                    minLength: minLength(10),
                    numeric,
                },
                fio: {
                    required
                },
                myData: {
                    required
                },
                phone: {
                    required,
                    minLength: minLength(10),
                    numeric,
                },
                email: {
                    required,
                    email,
                },
                pasport: {
                    required,
                    numeric,
                },
                pasdata: {
                    required
                },
                fms: {
                    required
                },
                adresRegistr: {
                    required
                },
            }
        },
        methods: {
            countDownChanged(dismissCountDown) {
                this.dismissCountDown = dismissCountDown
            },
            showAlert() {
                this.dismissCountDown = this.dismissSecs
            },
            submitHandler() {
                this.$v.$touch()
                if(!this.$v.$invalid) {
                    this.$axios.post('https://cdek-tumanova.ru/backend/public/api/dogovor-employed', {
                        inn: this.form.inn,
                        fio: this.form.fio,
                        myData: this.form.myData,
                        phone: this.form.phone,
                        email: this.form.email,
                        pasport: this.form.pasport,
                        pasdata: this.form.pasdata,
                        fms: this.form.fms,
                        adresRegistr: this.form.adresRegistr,
                        site: this.form.site || 'Не указано',
                        dopinfo: this.form.dopinfo || 'Не указано',
                    })
                        .then((response) => {
                            if(response.status === 200) {
                                for (let key in this.form) {
                                    this.form[key] = null
                                }
                                this.$v.$reset()
                                this.variant = 'success'
                                this.showAlert()
                            }
                        })
                        .catch((error) => {
                            this.variant = 'danger'
                            this.showAlert()
                        });
                }
            }
        },
    };
</script>
