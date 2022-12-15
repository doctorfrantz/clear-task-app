<template>
  <footer>
    <div class="footer-section">
      <div class="footer-logo">
        <img
          src="../assets/img/Logo.png"
          alt="ClearTask Logo"
        />
      </div>
      <div class="footer-separador"></div>
      <div class="footer-text">
        <p>Alexis Frantz - 2022</p>
      </div>
      <Dark />
    </div>
    <div class="footer-mobile">
      
        <ul class="footer-nav">
          <router-link to="/">
            <li class="footer-nav-item">
              <img class="footer-icon-img" src="../assets/img/Tasks.png" alt="Tasks Icon">Tasks
            </li>
          </router-link>
          <router-link to="/account">
            <li class="footer-nav-item">
              <img class="footer-icon-img" src="../assets/img/Account.png" alt="Account Icon">Account
            </li>
          </router-link>
          <div @click="signOut">
            <li class="footer-nav-item">
              <img class="footer-icon-img" src="../assets/img/LogOutGreen.png" alt="Log Out Icon">Log Out
            </li>
          </div>
        </ul>
      
    </div>
  </footer>
</template>

<script setup>
import { useRouter } from "vue-router";
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";
import Dark from "./Dark.vue";

const redirect = useRouter();

const signOut = async () => {
  try {
    //llamamos la tienda y enviamos al usuario al backendo para el signOut
    await useUserStore().signOut();
    //redirect del usuario al Auth view
    redirect.push({ path: "/auth/login" });
  } catch (error) {
    //mostramos el error message
    errorMsg.value = `Error: ${error.message}`;
    //ocultamos el errror
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
};
</script>

<style scoped>
</style>
