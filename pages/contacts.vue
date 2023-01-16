<template>
  <section class="section">
    <h2 class="mb-4">Контакты</h2>
    <div class="contacts d-lg-flex">
      <div class="contacts__info">
        <h5 class="contacts__subtitle">Пункт СДЭК SHCHLK11</h5>
        <p class="contacts__description">Мы находимся по адресу: ул. 8 Марта, 29 Щёлково, Московская обл., 141112</p>
        <div class="contacts-footer">
          <h5 class="contacts-footer__title mt-4">Наша команда</h5>
          <table class="contacts__table table">
            <tbody>
            <tr>
              <td><p class="mb-0">Позвони нам:</p></td>
              <td class="contacts__phone">
                <b-link href="tel:+79777602973">+7 977-760-29-73</b-link>
              </td>
              <td><a href="tel:+79777602973">
                <b-icon class="contacts__icon" icon="whatsapp" variant="info"></b-icon>
              </a></td>
            </tr>
            <tr>
              <td><p class="mb-0">Директор пвз:</p></td>
              <td colspan="2">Туманова Елена Владимировна</td>
            </tr>
            <tr>
              <td><p class="mb-0">Менеджер по работе с клиентами:</p></td>
              <td colspan="2">Киселев Алексей Владимирович</td>
            </tr>
            <tr>
              <td><p class="mb-0">Менеджер офиса:</p></td>
              <td colspan="2">Ткалич Федор Евгеньевич</td>
            </tr>
            <tr>
              <td><p class="mb-0">Менеджер клиентского отдела:</p></td>
              <td colspan="2">Кузовлева Полина Александровна</td>
            </tr>
            <tr>
              <td><p class="mb-0">Менеджер клиентского отдела:</p></td>
              <td colspan="2">Миронова Татьяна Николаевна</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
      <div class="contacts__media mt-5">
        <iframe allowfullscreen=""
                class="w-100 min-h-200"
                loading="lazy" referrerpolicy="no-referrer-when-downgrade"
                src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2236.597846045729!2d37.98378641609307!3d55.904333487762024!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x46b52d32ed7edf2b%3A0xb9c8fd18074a494!2z0YPQuy4gOCDQnNCw0YDRgtCwLCAyOSwg0KnRkdC70LrQvtCy0L4sINCc0L7RgdC60L7QstGB0LrQsNGPINC-0LHQuy4sIDE0MTExMg!5e0!3m2!1sru!2sru!4v1673736966375!5m2!1sru!2sru"
                style="border:0;"></iframe>
      </div>
    </div>
    <carousel-3d :autoplay="true"
                 :autoplay-timeout="3000"
                 :clickable="false"
                 :controls-height="30"
                 :controls-next-html="'&#x203A;'"
                 :controls-prev-html="'&#x2039; '"
                 :controls-visible="true"
                 :controls-width="10"
                  class="contacts-slider"
    >
      <slide class="contacts-slider__item" :index="0">
        <img src="~/static/media/office/img-1.jpg">
      </slide>
      <slide class="contacts-slider__item" :index="1">
        <img src="~/static/media/office/img-2.jpg">
      </slide>
      <slide class="contacts-slider__item" :index="2">
        <img src="~/static/media/office/img-3.jpg">
      </slide>
      <slide class="contacts-slider__item" :index="3">
        <img src="~/static/media/office/img-4.jpg">
      </slide>
      <slide class="contacts-slider__item" :index="4">
        <img src="~/static/media/office/img-5.jpg">
      </slide>
    </carousel-3d>
  </section>
</template>

<script>
export default {
  name: "contacts"

}
</script>

<style lang="scss" scoped>
@import "@/pages/styles/contacts/contacts.scss";
</style>
