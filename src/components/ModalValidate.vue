<template>
  <modal title="Modal with form" @close="$emit('close')">
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">
        <!-- Name -->
        <div class="form-item" :class="{ 'errorInput': $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required">Filed is required</p>
          <p
            class="errorText"
            v-if="!$v.name.minLength"
          >Name must have at least {{ $v.name.$params.minLength.min }}</p>
          <input v-model="name" :class="{ error: $v.name.$error}" @change="$v.name.$touch()">
        </div>
        <!-- Password -->
        <div class="form-item" :class="{ 'errorInput': $v.password.$error }">
          <label>Password:</label>
          <p class="errorText" v-if="!$v.password.required">Введите пароль</p>
          <p class="errorText" v-if="!$v.password.minLength">
            Пароль должен быть
            <b>{{ $v.password.$params.minLength.min }}</b> или более символов
          </p>
          <input
            v-model="$v.password.$model"
            :class="{ error: $v.password.$error}"
            @change="$v.password.$touch()"
          >
        </div>
        <!-- Password Сheck -->
        <div class="form-item" :class="{ 'errorInput': $v.repeatPassword.$error }">
          <label>Password check:</label>
          <p class="errorText" v-if="!$v.repeatPassword.required">Повторите пароль</p>
          <p class="errorText" v-if="this.password == this.repeatPassword">Пароли не совпадают!</p>
          <input
            v-model="$v.repeatPassword.$model"
            :class="{ error: $v.repeatPassword.$error}"
            @change="$v.repeatPassword.$touch()"
          >
        </div>
        <!-- Email -->
        <div class="form-item" :class="{ 'errorInput': $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required">Filed is required</p>
          <p class="errorText" v-if="!$v.email.email">Email is not correct</p>
          <input v-model="email" :class="{ error: $v.email.$error}" @change="$v.email.$touch()">
        </div>
        <!-- Button -->
        <button class="btn btnPrimary">Submit</button>
      </form>
    </div>
    <!-- /body -->
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from "vuelidate/lib/validators";

import modal from "@/components/UI/Modal.vue";
export default {
  components: {
    modal
  },
  data() {
    return {
      name: "",
      password: "",
      repeatPassword: "",
      check: true,
      email: ""
    };
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      required,
      sameAsPassword: sameAs("password")
    }
  },
  methods: {
    onSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const user = {
          name: this.name,
          email: this.email,
          password: this.password,
          repeatPassword: this.repeatPassword
        };
        console.log(user);

        // Done!
        this.name = "";
        this.email = "";
        this.password = "";
        this.repeatPassword = "";
        this.$v.$reset();
        this.$emit("close");
      }
    }
  }
};
</script>


<style lang="scss">
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4px;
  color: #de4040;
}
.form-item {
  &.errorInput .errorText {
    display: block;
  }
}
input.error {
  border-color: #de4040;
}
</style>