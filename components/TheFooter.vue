<template>
  <footer class="footer">
    <a href="mailto:abc@example.com" class="mail-address">Send me an E-Mail!</a>
    <p>2020 Nectoj.io</p>
  </footer>
</template>

<style scoped>
.footer {
  display: flex;
  justify-content: space-between;
  padding-top: 15px;
  flex-shrink: 0;
}

.footer > a,
p {
  padding: 0px 20px 0px 20px;
}

.mail-address,
.mail-address:active {
  text-decoration: none;
  color: #6631ff;
}
</style>