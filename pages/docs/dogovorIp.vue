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
        <h2>Анкета для заключения договора</h2>
        <b-form class="mt-4" @submit.prevent="submitHandler">
            <b-form-group
                label="ФИО (индивидуального предпринимателя):"
                label-for="input-1"
            >
                <b-form-input
                    id="input-1"
                    type="text"
                    placeholder="Введите ФИО"
                    v-model="$v.form.fio.$model"
                    :state="$v.form.fio.$dirty ? !$v.form.fio.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.fio.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="ИНН:"
                label-for="input-2"
            >
                <b-form-input
                    id="input-2"
                    type="text"
                    placeholder="Введите номер"
                    v-model="$v.form.inn.$model"
                    :state="$v.form.inn.$dirty ? !$v.form.inn.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.inn.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.inn.numeric && $v.form.inn.required">Должны быть только цифры</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.inn.minLength && $v.form.inn.required && $v.form.inn.numeric">Это поле не должно быть меньше 10 цифр</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group label="Фактический адрес отличается?" v-slot="{ ariaDescribedby }">
                <b-form-radio v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios" value="Отличается">Отличается</b-form-radio>
                <b-form-radio v-model="selected" :aria-describedby="ariaDescribedby" name="some-radios" value="Не отличается">Не отличается</b-form-radio>
            </b-form-group>
            <b-form-group
                label="Фактический адрес:"
                label-for="input-3"
                v-if="selected === 'Отличается'"
            >
                <b-form-input
                    id="input-3"
                    type="text"
                    placeholder="Город, улица, дом/офис"
                    v-model="$v.form.adress.$model"
                    :state="$v.form.adress.$dirty ? !$v.form.adress.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.adress.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="Номер свидетельства (не обязательно)"
                label-for="input-4"
            >
                <b-form-input
                    id="input-4"
                    type="text"
                    placeholder="Введите номер"
                    v-model="form.numSvid"
                ></b-form-input>
            </b-form-group>
            <b-form-group
                label="Дата выдачи свидетельства (не обязательно)"
                label-for="input-5"
            >
                <b-form-datepicker
                    id="input-5"
                    v-model="form.dateSvid"
                    placeholder="Выберите дату"
                ></b-form-datepicker>
            </b-form-group>
            <b-form-group
                label="БИК банка:"
                label-for="input-6"
            >
                <b-form-input
                    id="input-6"
                    type="text"
                    placeholder="Введите БИК"
                    v-model="$v.form.bik.$model"
                    :state="$v.form.bik.$dirty ? !$v.form.bik.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.bik.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.bik.numeric && $v.form.bik.required">Должны быть только цифры</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.bik.minLength && $v.form.bik.required && $v.form.bik.numeric">Это поле не должно быть меньше 9 цифр</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="ФИО заполнителя анкеты:"
                label-for="input-7"
            >
                <b-form-input
                    id="input-7"
                    type="text"
                    placeholder="Введите ФИО"
                    v-model="$v.form.fioAutor.$model"
                    :state="$v.form.fioAutor.$dirty ? !$v.form.fioAutor.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.fioAutor.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="ФИО директора:"
                label-for="input-8"
            >
                <b-form-input
                    id="input-8"
                    type="text"
                    v-model="$v.form.fioDirector.$model"
                    :state="$v.form.fioDirector.$dirty ? !$v.form.fioDirector.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.fioDirector.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="Основания действий директора:"
                label-for="input-9"
            >
                <b-form-input
                    id="input-9"
                    type="text"
                    v-model="$v.form.osnovanija.$model"
                    :state="$v.form.osnovanija.$dirty ? !$v.form.osnovanija.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.osnovanija.required">Это поле не должно быть пустым</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="Телефон:"
                label-for="input-10"
            >
                <b-form-input
                    id="input-10"
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
                label-for="input-11"
            >
                <b-form-input
                    id="input-11"
                    type="text"
                    placeholder="Введите email"
                    v-model="$v.form.email.$model"
                    :state="$v.form.email.$dirty ? !$v.form.email.$invalid : null"
                ></b-form-input>
                <b-form-invalid-feedback v-if="!$v.form.email.required">Это поле не должно быть пустым</b-form-invalid-feedback>
                <b-form-invalid-feedback v-if="!$v.form.email.email && $v.form.email.required">Не корректный email</b-form-invalid-feedback>
            </b-form-group>
            <b-form-group
                label="Сайт (не обязательно)"
                label-for="input-12"
            >
                <b-form-input
                    id="input-12"
                    type="text"
                    placeholder="Введите ссылку на ваш сайт"
                    v-model="form.site"
                ></b-form-input>
            </b-form-group>
            <b-form-group
                label="Комментарий (не обязательно)"
                label-for="input-13"
            >
                <b-form-textarea
                    id="input-13"
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
        name: 'dogovorIp',
        data () {
            return {
                dismissSecs: 5,
                dismissCountDown: 0,
                variant: '',
                selected: 'Не отличается',
                form: {
                    fio: null,
                    inn: null,
                    adress: null,
                    numSvid: null,
                    dateSvid: null,
                    bik: null,
                    fioAutor: null,
                    fioDirector: null,
                    osnovanija: null,
                    phone: null,
                    email: null,
                    site: null,
                    dopinfo: null,
                }
            }
        },
        validations: {
            form: {
                fio: {
                    required
                },
                inn: {
                    required,
                    minLength: minLength(10),
                    numeric,
                },
                adress: {
                    required
                },
                bik: {
                    required,
                    minLength: minLength(9),
                    numeric,
                },
                fioAutor: {
                    required,
                },
                fioDirector: {
                    required,
                },
                osnovanija: {
                    required,
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
                if(this.selected === 'Не отличается') {
                    this.form.adress = 'Не отличается'
                }
                this.$v.$touch()
                if(!this.$v.$invalid) {
                    this.$axios.post('https://cdek-tumanova.ru/backend/public/api/dogovor-ip', {
                        fio: this.form.fio,
                        inn: this.form.inn,
                        adress: this.form.adress,
                        numSvid: this.form.numSvid || 'Не указано',
                        dateSvid: this.form.dateSvid || 'Не указано',
                        bik: this.form.bik,
                        fioAutor: this.form.fioAutor,
                        fioDirector: this.form.fioDirector,
                        osnovanija: this.form.osnovanija,
                        phone: this.form.phone,
                        email: this.form.email,
                        site: this.form.site || 'Не указано',
                        dopinfo: this.form.dopinfo || 'Не указано',
                    })
                    .then((response) => {
                        if(response.status === 200) {
                            this.selected = 'Не отличается'
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

        watch: {
            selected: {
                handler() {
                    this.selected === 'Не отличается' ? this.form.adress = 'Не отличается' : this.form.adress = null
                },
            },
        }
    };
</script>
