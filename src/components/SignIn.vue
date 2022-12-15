<template>
  <div class="container">
    <div class="form-side">
      <div class="header">
        <a href="/auth/#" class="header-logo">
          <img class="navbar-img" src="../assets/img/Logo.png" alt="Logo Clear Task" />
        </a>
        <div class="add-task-section">
          <h3 class="add-task-title title">Log In to Clear Task</h3>
          <p class="add-task-quote">Help your productivity by organizing your tasks</p>
        </div>
      </div>

      <p v-show="errorMsg" class="error-msg">{{ errorMsg }}</p>

      <form @submit.prevent="signIn" class="form-sign-in">
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
              :type="passwordFieldVisibility"
              class="input-field"
              placeholder="*********"
              id="password"
              v-model="password"
              required
            />
            <i
              v-if="showPassword"
              @click="togglePasswordVisibility"
            ><img src="../assets/img/dontShowPassword.png" alt="Don't Show Password"></i>
            <i
              v-if="!showPassword"
              @click="togglePasswordVisibility"
            ><img src="../assets/img/showPassword.png" alt="Show Password"></i>
          </div>
          <button class="sign-in-button">Sign In</button>
          <p class="account-text">
            Dont have an account?
            <PersonalRouter
              :route="route"
              :buttonText="buttonText"
              class="sign-up-link"
            />
          </p>
        </div>
      </form>
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


const route = "/auth/sign-up";
const buttonText = "Sign Up";

const email = ref("");
const password = ref("");

const errorMsg = ref("");

const passwordFieldVisibility = computed(() =>
  showPassword.value ? "password" : "text"
);
const showPassword = ref(true);

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

const redirect = useRouter();

const signIn = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);
    redirect.push({ path: "/" });
  } catch (error) {
    errorMsg.value = `Error: ${error.message}`;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>

<style scoped>
</style>
