<template>
  <div class="text-center">
    <form class="form-signin" @submit.prevent="userRegister">
      <h1 class="h3 mb-3 mt-3 font-weight-normal">Введите данные для изменения профиля</h1>
      <label for="inputUsername" class="sr-only">Ваше пользователя {{user.user_info.username}}</label>
      <input id="inputUsername" class="form-control" placeholder="Новое имя пользователя" required="" v-model="register.username">
      <label for="inputFirstName" class="sr-only">Ваше имя {{user.user_info.first_name}}</label>
      <input id="inputFirstName" class="form-control" placeholder="Новое имя" required="" v-model="register.first_name">
      <label for="inputLastName" class="sr-only">Ваша фамилия {{user.user_info.last_name}}</label>
      <input id="inputLastName" class="form-control" placeholder="Новая фамилия" required="" v-model="register.last_name">
      <label for="inputPassword" class="sr-only">Ваш email {{user.user_info.email}}</label>
      <input id="inputLastName" class="form-control" placeholder="Новый email" required="" v-model="register.email">
      <label for="inputPassword" class="sr-only">Новый пароль</label>
      <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Пароль" required="" v-model="register.password">
      <label for="ReInputPassword" class="sr-only">Повторите пароль</label>
      <input type="password" id="ReInputPassword" class="form-control mt-2" placeholder="Повторите пароль" required="" v-model="register.confirm_password">
      <button class="btn mt-2 btn-lg btn-primary btn-block" type="submit">Обновить</button>
    </form>
  </div>
</template>

<script>
export default {
  layout: "post_detail",
  data() {
    return {
      register: {
        username: '',
        first_name: '',
        last_name: '',
        email: '',
        password: '',
        confirm_password: '',
      },
    }
  },
  methods: {
    async userRegister() {
      try {
        let response = await this.$axios.post('/blogapi/profile_edit/', {
          username: this.register.username,
          first_name: this.register.first_name,
          last_name: this.register.last_name,
          email: this.register.email,
          password: this.register.password,
          confirm_password: this.register.confirm_password
        })
        console.log(response)
        await this.$auth.loginWith('local', {
          data: {
            username: this.register.username,
            password: this.register.password
          },
        })
        this.$router.push('/')
      } catch (err) {
        console.log(err)
      }
    }
  },
  computed: {
    user() {
      return this.$auth.user
    },
  },
}
</script>

<style scoped>

</style>