<template>
  <div @click="toggle(!isDarkMode)">
    <i v-if="isDarkMode"><img class="switch-img" src="../assets/img/switchOn.png" alt="ON"></i>
    <i v-if="!isDarkMode"><img class="switch-img" src="../assets/img/switchOff.png" alt="OFF"></i>
  </div>
</template>

<script>
export default {
    data() {
        return {
            isDarkMode: false,
        }
    },
    methods: {
        toggle(payload) {
            this.isDarkMode = payload
            document.documentElement.className = payload
            localStorage.setItem('theme', payload)
        }
    }
}
</script>