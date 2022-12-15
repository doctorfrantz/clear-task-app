<template>
  <div class="container">
    <div class="form-side">
      <div class="header">
        <a href="/auth/#" class="header-logo">
          <img class="navbar-img" src="../assets/img/Logo.png" alt="Logo Clear Task" />
        </a>
        <div class="add-task-section">
          <h3 class="add-task-title title">Register to Clear Task</h3>
          <p class="add-task-quote">Help your productivity by organizing your tasks</p>
        </div>
      </div>

      <form @submit.prevent="signUp" class="form-sign-in">
        <div class="form">
          <div class="form-logo"></div>
          <div class="form-input">
            <label class="input-field-label">
              <img src="../assets/img/Email.png" alt="Email Icon"> E-mail
            </label>
            <input
              type="email"
              class="input-field"
              placeholder="example@gmail.com"
              id="email"
              v-model="email"
              required
            />
          </div>
          <div class="form-input">
            <label class="input-field-label">
              <img src="../assets/img/Password.png" alt="Password Icon"> Password
            </label>
            <input
              type="password"
              class="input-field"
              placeholder="**********"
              id="password"
              v-model="password"
              required
            />
          </div>
          <div class="form-input">
            <label class="input-field-label">
              <img src="../assets/img/Password.png" alt="Password Icon"> Comfirm password
            </label>
            <input
              type="password"
              class="input-field"
              placeholder="**********"
              id="confirmPassword"
              v-model="confirmPassword"
              required
            />
          </div>
          <button class="sign-in-button" type="submit">Sign Up
          </button>
          <p class="account-text">
            Have an account?
            <PersonalRouter
              :route="route"
              :buttonText="buttonText"
              class="sign-up-link"
            />
          </p>
        </div>
      </form>

      <div v-show="errorMsg" class="error-password">{{ errorMsg }}</div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";


const route = "/auth/login";
const buttonText = "Sign In";

const email = ref("");
const password = ref("");
const confirmPassword = ref("");

const errorMsg = ref("");

const redirect = useRouter();

const passwordFieldVisibility = computed(() =>
  showPassword.value ? "password" : "text"
);
const showPassword = ref(true);

const confirmPasswordFieldVisibility = computed(() =>
  hideConfirmPassword.value ? "password" : "text"
);
const hideConfirmPassword = ref(true);

const signUp = async () => {
  if (password.value === confirmPassword.value) {
    try {
      await useUserStore().signUp(email.value, password.value);
      redirect.push({ path: "/auth/login" });
    } catch (error) {
      errorMsg.value = "Password do not match";
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    }
    return;
  }
  errorMsg.value = "error";
};
</script>

<style scoped>
</style>
