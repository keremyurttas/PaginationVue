<template>
  <section class="container">
    <button disabled class="button-disabled">&lt;</button>
    <button class="page page-active">1</button>

    <button class="page page-disabled">2</button>
    <button class="button-active">&gt;</button>
  </section>
</template>
<script></script>
<style>
:root {
  --color-page-active: #4200ff;
  --color-primary: #c4cdd5;
  --bg: #dfe3e8;
  --bg-primary: white;
  --bg-button-disabled: #919eab;
}
.container {
  padding: 10rem;

  width: 100%;
  background-color: var(--bg);
  display: flex;
  gap: 8px;
  align-items: center;
  justify-content: center;
}
button {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 2rem;
  min-height: 2rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}
button:not(.button-disabled):hover {
  transform: scale(1.05);
}
.button-active {
  background: var(--bg-primary);
  color: var(--color-primary);
  font-size: x-large;
}
.button-disabled {
  background: var(--bg-button-disabled);
  color: var(--color-primary);
  font-size: x-large;
  cursor: default;
}
.page {
  font-weight: bold;
  background: var(--bg-primary);
}
.page-active {
  border-color: var(--color-page-active);
  color: var(--color-page-active);
  border: solid 1px;
}
.page-disabled {
  background-color: var(--bg-primary);
}
img {
  box-shadow: none;
}
</style>
