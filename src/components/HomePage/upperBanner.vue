<template>
  <div class="upper-banner">
    <div class="container">
      <div class="left">
        <h1>
          <span> Huge Saving</span> on <br />
          UHD Televisions
        </h1>
        <p>Sale up to 70% off on selected items*</p>
        <button class="btn btn-outline-light rounded-pill px-3">
          Shop Now
        </button>
      </div>
      <div class="bg-img">
        <img
          src="@/assets/ella_imgs/banner-bg-screen.png"
          width="100%"
          alt="upper banner"
        />
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.upper-banner {
  color: #fff;
  background-image: url(../../assets/ella_imgs/banner-bg-Recovered.png);
  background-repeat: no-repeat;
  background-size: cover;
  .container {
    display: flex;
    align-items: center;
    justify-content: space-around;
    flex-wrap: wrap;
    padding-top: 30px;
    .left {
      h1 {
        font-weight: bold;
        font-size: 45px;
        span {
          color: #10ffda;
        }
      }
      .btn {
        padding: 0.5rem 2rem;
        transition: 0.5s;
      }
    }
  }
  @media (max-width: 991px) {
    padding-top: 50px;
  }
  @media (min-width: 992px) {
    padding-top: 170px;
  }
}
</style>
