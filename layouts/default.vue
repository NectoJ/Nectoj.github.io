<template>
  <div>
    <TheHeader />
    <div class="spacing">
      <main class="content">
        <nuxt />
      </main>
    </div>
    <TheFooter />
  </div>
</template>

<style>
.content {
  display: flex;
  flex: 1;
  flex-direction: row;
  min-width: 20em;
  padding: 1em 1.5em 1em;
  background-color: #ffffff;
  color: #000000;
  border-radius: 20px;
}

.spacing {
  display: flex;
  height: auto;
}
</style>