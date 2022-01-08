<template>
  <div class="hero">
    <img src="../assets/img/movieHero.jpg" alt="hero-movie">
    <div class="text-container">
      <div class="text">
        <span class="mini-heading">
        <h1><span>Now</span> Streaming</h1>
        <a href="#movies-grid" class="button">View Movies</a>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'Hero',
}
</script>

<style lang="scss">
.hero {
  height: 400px;
  position: relative;

  @media (min-width: 750px) {
    height: 500px;
  }
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.2);
  }
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .text-container {
    z-index: 99;
    position: absolute;
    top: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding:10px 4rem;
    .text {
      padding: 0 16px;
      width: 100%;
      max-width: 1400px;
      margin: 0 auto;
    }
    .mini-heading {
      font-weight: 600;
      font-size: 18px;
      text-transform: uppercase;
      color: #fff;
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 22px;
      }
    }
    h1 {
      color: #fff;
      font-size: 64px;
      font-weight: 200;
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 84px;
      }
      span {
        font-weight: 500;
      }
    }
    .button {
      font-size: 20px;
      align-self: flex-start;
      background-color:#11ae91;
    }
  }
}
</style>