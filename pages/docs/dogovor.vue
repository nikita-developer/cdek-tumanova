<template>
  <section class="section">
    <b-alert
      v-if="variant === 'success'"
      :show="dismissCountDown"
      variant="success"
      @dismissed="dismissCountDown=0"
      @dismiss-count-down="countDownChanged"
      class="mt-5"
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
      variant="danger"
      @dismissed="dismissCountDown=0"
      @dismiss-count-down="countDownChanged"
      class="mt-5"
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
    <b-form class="mt-4" @submit.prevent.stop="submitHandler">
      <b-form-group
        label="Ваша организация:"
        label-for="input-1"
      >
        <b-form-input
          id="input-1"
          type="text"
          placeholder="ООО, ИП или самозанятый"
          v-model.trim="form.organization.text"
          :state="form.organization.status"
          @input="validate()"
        ></b-form-input>
        <b-form-invalid-feedback>{{ form.organization.error }}</b-form-invalid-feedback>
      </b-form-group>
      <b-form-group
        label="Ваш номер телефона:"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          type="tel"
          placeholder="8 999 999 99 99"
          v-model.trim="form.phone.text"
          :state="form.phone.status"
          @input="validate()"
        ></b-form-input>
        <b-form-invalid-feedback>{{ form.phone.error }}</b-form-invalid-feedback>
      </b-form-group>
      <b-form-group
        label="Ваш ИНН:"
        label-for="input-3"
      >
        <b-form-input
          id="input-3"
          type="text"
          placeholder="ИНН..."
          v-model.trim="form.inn.text"
          :state="form.inn.status"
          @input="validate()"
        ></b-form-input>
        <b-form-invalid-feedback>{{ form.inn.error }}</b-form-invalid-feedback>
      </b-form-group>
      <b-form-group
        label="Ваш E-mail:"
        label-for="input-4"
      >
        <b-form-input
          id="input-4"
          type="email"
          placeholder="mail@mail.ru"
          v-model.trim="form.email.text"
          :state="form.email.status"
          @input="validate()"
        ></b-form-input>
        <b-form-invalid-feedback>{{ form.email.error }}</b-form-invalid-feedback>
      </b-form-group>
      <b-form-group
        label="Ваш сайт:"
        label-for="input-5"
      >
        <b-form-input
          id="input-5"
          type="text"
          placeholder="Адрес вашего сайта"
          v-model.trim="form.site.text"
          @input="validate()"
        ></b-form-input>
      </b-form-group>
      <b-form-group label="Ваш тип договора:" label-for="input-6">
        <b-form-select
          id="input-6"
          v-model="form.docs.text"
          :options="['С обычным договором', 'С договором интернет-магазина']"
          required
        ></b-form-select>
      </b-form-group>
      <b-form-group
        label="Дополнительная информация:"
        label-for="input-7"
      >
        <b-form-textarea
          id="input-7"
          placeholder="Введите текст..."
          rows="6"
          v-model.trim="form.dopinfo.text"
        ></b-form-textarea>
      </b-form-group>
      <b-button type="submit" class="mt-4" block variant="primary">Отправить</b-button>
    </b-form>
  </section>
</template>

<script>
export default {
  name: "dogovor",
  data() {
    return {
      form: {
        organization: {
          text: '',
          status: null,
          error: '',
        },
        phone: {
          text: '',
          status: null,
          error: '',
        },
        inn: {
          text: '',
          status: null,
          error: '',
        },
        email: {
          text: '',
          status: null,
          error: '',
        },
        site: {
          text: '',
          status: true,
          error: '',
        },
        docs: {
          text: 'С обычным договором',
          status: null,
          error: '',
        },
        dopinfo: {
          text: '',
          status: true,
          error: '',
        },
      },
      dismissSecs: 5,
      dismissCountDown: 0,
      variant: '',
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
      if(this.validate()) {
        this.$axios.post('https://cdek-tumanova.ru/backend/public/api/dogovor', {
          organization: this.form.organization.text,
          phone: this.form.phone.text,
          inn: this.form.inn.text,
          email: this.form.email.text,
          site: this.form.site.text || 'Нет сайта',
          docs: this.form.docs.text,
          dopinfo: this.form.dopinfo.text || 'Договорник решил не писать дополнительную информацию',
        })
          .then((response) => {
            if(response.status === 200) {
              this.form.organization.text = ''
              this.form.organization.status = null
              this.form.phone.text = ''
              this.form.phone.status = null
              this.form.inn.text = ''
              this.form.inn.status = null
              this.form.email.text = ''
              this.form.email.status = null
              this.form.site.text = ''
              this.form.docs.text = 'С обычным договором'
              this.form.docs.status = null
              this.form.dopinfo.text = ''
              this.variant = 'success'
              this.showAlert()
            }
          })
          .catch((error) => {
            this.variant = 'danger'
            this.showAlert()
          });
      }
    },
    validate() {
      let regularMail = /^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/;
      let regularPhone = /^(\s*)?(\+)?([- _():=+]?\d[- _():=+]?){10,14}(\s*)?$/;
      let regularInn = /^(\d{10}|\d{12})$/;
      let submit = false;

      if(this.form.organization.text) {
        this.form.organization.status = true
        this.form.organization.error = ''
      } else {
        this.form.organization.status = false
        this.form.organization.error = 'Это поле не должно быть пустым'
      }

      if(this.form.phone.text) {
        if(!regularPhone.test(this.form.phone.text)) {
          this.form.phone.status = false
          this.form.phone.error = 'Введите корректный телефон'
        } else {
          this.form.phone.status = true
          this.form.phone.error = ''
        }
      } else {
        this.form.phone.status = false
        this.form.phone.error = 'Это поле не должно быть пустым'
      }

      if(this.form.inn.text) {
        if(!regularInn.test(this.form.inn.text)) {
          this.form.inn.status = false
          this.form.inn.error = 'Введите корректный ИНН'
        } else {
          this.form.inn.status = true
          this.form.inn.error = ''
        }
      } else {
        this.form.inn.status = false
        this.form.inn.error = 'Это поле не должно быть пустым'
      }

      if(this.form.email.text) {
        if(!regularMail.test(this.form.email.text)) {
          this.form.email.status = false
          this.form.email.error = 'Введите корректный e-mail'
        } else {
          this.form.email.status = true
          this.form.email.error = ''
        }
      } else {
        this.form.email.status = false
        this.form.email.error = 'Это поле не должно быть пустым'
      }

      if(this.form.docs.text) {
        this.form.docs.status = true
        this.form.docs.error = ''
      } else {
        this.form.docs.status = false
        this.form.docs.error = 'Это поле не должно быть пустым'
      }

      for (let key in this.form) {
        if(this.form[key].status) {
          submit = true
        } else {
          return submit = false
        }
      }
      return submit
    },
  },
}
</script>

<style scoped>

</style>
