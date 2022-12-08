<template>
  <img src="~assets/wave.png" class="wave">
  <div class="row" style="height: 90vh">
    <div class="col-0 col-md-6 flex justify-center content-center">
      <img src="~assets/login.svg" class="responsive">
    </div>
    <div v-bind:class="{ 'justify-center': $q.screen.md || $q.screen.sm || $q.screen.xs }"
         class="col-12 col-md-6 flex content-center">
      <q-card v-bind:style="$q.screen.lt.sm ? { 'width': '80%' } : {'width': '50%'}">
        <q-card-section>
          <q-avatar size="103px" class="absolute-center shadow-10">
            <img src="~assets/avatar.svg" alt="avatar">
          </q-avatar>
        </q-card-section>
        <q-card-section>
          <div class="q-pt-lg">
            <div class="col text-h6 ellipsis flex justify-center">
              <h2 class="text-h2 q-my-none text-weight-regular">
                Войти
              </h2>
            </div>
          </div>
        </q-card-section>
        <q-card-section>
          <q-form class="q-gutter-md" @submit.prevent="submitForm">

            <q-input label="Логин" v-model="login.username"></q-input>
            <q-input label="Пароль" type="password" v-model="login.password"></q-input>

            <div>
              <q-btn class="full-width" color="primary" label="Войти" type="submit" rounded></q-btn>
              <div class="text-center q-mt-sm q-gutter-lg">
                <router-link class="text-white" to="#">Не удается войти?</router-link>
                <router-link class="text-white" to="#">Регистрация</router-link>
              </div>
            </div>

          </q-form>
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<script>
import { useQuasar } from 'quasar'
import { mapActions } from 'vuex'

let $q;

export default {
  name: "Login",
  data()
  {
    return {
      login: {
        username: "Misha",
        password: "123456"
      }
    };
  },
  methods:
  {
    ...mapActions('auth', ['doLogin']),

      async submitForm()
      {
        if(!this.login.username || !this.login.password)
        {
          $q.notify({
            type: 'negative',
            message: 'Логин или пароль обязательны для заполнения'
          })
        }
        else if(this.login.password.length < 6)
        {
          $q.notify({
            type: 'negative',
            message: 'Пароль должен быть больше 5 символов'
          })
        }
        else
        {
          try
          {
            await this.doLogin(this.login);
            await this.$router.push('/')
          }
          catch (error)
          {
            console.log(error);
            $q.notify({
              type: 'negative',
              message: error.response.data.value
                ? error.response.data.value
                : 'Произошла непредвиденная ошибка!'
            })
          }
        }
      }
  },
  mounted()
  {
    $q = useQuasar()
  }
};
</script>

<style scoped>
.wave {
  position: fixed;
  height: 100%;
  left: 0;
  bottom: 0;
  z-index: -1;
}
</style>
