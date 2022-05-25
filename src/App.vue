<template>
  <header>
    <h1>anichek</h1>
  </header>
  <main>
    <router-view />
  </main>
</template>

<style lang="scss">
@import "./styles/main.scss";

header {
  h1 {
    color: var(--cl-text);
    font-weight: 800;
    font-size: 38px;
    text-align: center;
    margin: 20px auto 10px;
  }
}
</style>
