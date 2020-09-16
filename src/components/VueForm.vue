<template>
  <div class="container">
    <form
      @input="validator"
      @click="validator"
      v-if="!this.isLoggedIn"
      name="form"
      class="form"
    >
      <input
        v-model="email"
        type="email"
        name="email"
        placeholder="Введите пожалуйста ваш email"
        class="form__input form__input_email"
        pattern="[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z0-9]+"
        required
      />
      <input
        v-model="password"
        type="password"
        name="password"
        placeholder="Введите пожалуйста ваш пароль"
        class="form__input form__input_password"
        required
      />
      <button type="submit" @click="logIn" class="button form__button" disabled>
        Войти
      </button>
    </form>
    <div v-if="this.isLoggedIn" class="is-logged-container">
      <img src="@/assets/Meme.jpg" alt="Мемасик" class="meme" />
      <button @click="logOut" class="button">Выйти</button>
    </div>
  </div>
</template>

<script>
const bcrypt = require("bcryptjs");

export default {
  data: function() {
    return {
      email: "",
      password: "",
      user: { email: "gora@studio.ru", password: "2020" },
      isLoggedIn: false,
    };
  },

  mounted() {
    this.checkLogin(this.user);
  },

  methods: {
    checkLogin(data) {
      if (localStorage.user) {
        const userInStorage = JSON.parse(localStorage.user);
        if (userInStorage.email == data.email) {
          bcrypt
            .compare(data.password, userInStorage.password)
            .then((matched) => {
              if (matched) {
                return (this.isLoggedIn = true);
              } else {
                return (this.isLoggedIn = false);
              }
            });
        } else {
          return (this.isLoggedIn = false);
        }
      } else {
        return (this.isLoggedIn = false);
      }
    },
    logIn() {
      event.preventDefault();
      bcrypt
        .hash(this.password, 10)
        .then((hash) => {
          localStorage.setItem(
            "user",
            JSON.stringify({ email: this.email, password: hash })
          );
        })
        .then(() => this.checkLogin(this.user))
        .catch((err) => {
          console.log(err);
        });
    },
    logOut() {
      localStorage.removeItem("user");
      this.checkLogin(this.user);
    },
    checkInputValidity(element) {
      if (!element.checkValidity()) {
        return false;
      }
      return true;
    },
    setSubmitButtonState(result, element) {
      const button = element.parentElement.querySelector(".form__button");
      if (result) {
        button.classList.remove("button_disabled");
        button.removeAttribute("disabled");
      } else {
        button.classList.add("button_disabled");
        button.setAttribute("disabled", true);
      }
    },
    validator() {
      const element = document.forms.form;
      this.setSubmitButtonState(this.checkInputValidity(element), element);
    },
  },
};
</script>

<style scoped>
.container {
  width: 80%;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-width: 50%;
  min-height: 300px;
  padding: 10px;
  border-radius: 10px;
  background: #f5f5f7;
}

.form__input {
  width: 50%;
  margin-bottom: 30px;
  border: 1px solid #d6d6d6;
  border-radius: 4px;
  background: hsla(0, 0%, 100%, 0.8);
  background-clip: padding-box;
  padding: 5px;
}

.button {
  padding: 5px 20px;
}

.meme {
  max-width: 100%;
  margin-bottom: 50px;
}

.is-logged-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
