<template>
  <nav>
    <div class="header-mobile">
      <router-link to="/"><img src="../assets/img/Logo.png" alt="ClearTask Logo" class="header-mobile-logo"></router-link>
      <Dark />
    </div>
    <div class="nav-bar">
      <!-- <PersonalRouter :route="route" :buttonText="buttonText" class="logo-link"/> -->
      <router-link to="/">
        <img
          class="navbar-img animate__animated animate__bounceIn"
          src="../assets/img/Logo.png"
          alt="Logo"
        />
      </router-link>

      <ul class="log-out-elements navbar animate__animated animate__bounceIn">
        <li>
          <router-link to="/">Task Manager</router-link>
        </li>

        <li>
          <router-link to="/account">Your Account</router-link>
        </li>
      </ul>
      
      <div class="log-out">
        <ul class="log-out-elements animate__animated animate__bounceIn">
          <li class="log-out-welcome">
            <p>
              Welcome <span class="usuario-name">{{ emailName[0] }}</span>
            </p>
          </li>

          <li>
            <button @click="signOut" class="sign-out-button">
              <span class="button-bold"><p>Log Out</p><img src="../assets/img/LogOutWhite.png" alt="Log Out"></span>
            </button>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from "vue";import Dark from "./Dark.vue";

const route = "/";
const buttonText = "Todo app";

const getUser = useUserStore().user;

const userEmail = getUser.email;
const emailName = userEmail.split("@");

const redirect = useRouter();

const signOut = async () => {
  try {
    await useUserStore().signOut();
    redirect.push({ path: "/auth/login" });
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
