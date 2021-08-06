<template>
  <q-page class="flex flex-center">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->

<div class="q-pa-md" style="max-width: 400px">

    <q-form
      class="q-gutter-md"
    >
      <q-input
        v-model="login"
        filled
        label="Имя пользователя "
        hint="введите свой логин"
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'введите корректный логин']"
      />
      <q-input
        type="password"
        v-model="password"
        filled
        label="Пароль "
        hint="введите свой пароль"
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'введите корректный пароль']"
      />

      <q-input
        v-model="ip"
        @keyup.enter="setups"
        filled
        label="IP адрес устройства"
        hint="введите свой IP"
        lazy-rules
        :rules="[val => val && val.length > 0 || 'Введите корректный IP']"
      />

      <q-toggle v-model="accept" label="Сохранить данные для входа" />

      <div>
        <q-btn @click="setups"  label="Войти" type="submit" color="primary" />
        <!-- <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" /> -->
      </div>
    </q-form>

  </div>
  </q-page>
</template>

<script>
// import { defineComponent } from 'vue'
import { openURL, LocalStorage } from 'quasar'

// export default defineComponent({
//   name: 'PageIndex'
// })

export default {
  data () {
    if (LocalStorage.getItem('storage')) {
      const { login, password, ip, accept } = LocalStorage.getItem('storage')
      return {
        login,
        password,
        ip,
        accept
      }
    }
    return {
      login: 'root',
      password: 'segnetics',
      ip: '',
      accept: false

    }
  },
  methods: {
    setups () {
      if (this.accept) {
        LocalStorage.set('storage',
          {
            login: this.login,
            password: this.password,
            ip: this.ip,
            accept: this.accept
          })
      } else {
        LocalStorage.remove('storage')
      }

      console.log(LocalStorage.getItem('storage'))
      openURL(
        'http://' + this.ip + '/login.php?login=' + this.login + '&password=' + this.password,
        // 'http://',
        null, // in this example we don't care about the rejectFn()

        // this is the windowFeatures Object param:
        {
          noopener: true, // this is set by default for security purposes

          menubar: true,
          toolbar: true,
          noreferrer: true
          // .....any other window features
        }
      )
    },
    con () {
      console.log(this.accept)
    }
  }

}
</script>
