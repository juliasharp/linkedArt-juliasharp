<template>
  <footer class="footer">
    <div>
      <p>Built by JULIA SHARP</p>
      <p>Using <a href="https://linkedartjs.org" target="_blank" rel="noopener noreferrer">linkedart.js</a></p>
    </div>
  </footer>
</template>

<style lang="css" scoped>
.footer {
  position: absolute;
}
@media (max-width: 1023px) {
  .footer {
    bottom: 0;
    right: 0;
    padding: 10px 12px;
    font-size: 15px;
    text-align: center;
  }
  .footer  > div {
    display: flex;
  }
  .footer > div p:first-of-type {
    margin-right: 3px;
  }
} 
@media (min-width: 1024px) {
  .footer {
    left: 0;
    bottom: 0;
    color: white;
    padding: 10px 12px;
    font-size: 15px;
    text-align: center;
  }
}
</style>