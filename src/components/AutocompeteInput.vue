<template>
  <div class="Ui-container">
    <div class="google-Gsuite">
      <div class="right-div">
        <img src="../assets/down-filled-triangular-arrow.png" alt="" />
        <h4>Google GSuite Expense</h4>
      </div>

      <div class="left-img">
        <img src="../assets/i.png" alt="" />
        <img src="../assets/dots.png" alt="" />
      </div>
    </div>

    <!-- // error section -->

    <div class="error-section">
      <div>
        <img src="../assets/warning.png" alt="" />
        <h1>#Error</h1>
      </div>

      <div class="error-section-right">
        <div>Jul 2023</div>
      </div>
    </div>

    <!-- input section  -->

    <div class="input">
      <inputs />
    </div>

    <div class="add-time-containe">
      <div class="span-div">
        <span>
          <img src="../assets/plus (1).png" alt="" /> Add Time Segment</span
        >
      </div>
    </div>

    <div class="content-div">
      <div class="google-Gsuite">
        <div class="right-divs">
          <img src="../assets/right-arrow (1).png" alt="" />
          <h4>Hardware Expense</h4>
        </div>

        <div class="left-img">
          <img src="../assets/i.png" alt="" />
          <img src="../assets/dots.png" alt="" />
        </div>
      </div>

      <!-- // error section -->

      <div class="error-sections">
        <div class="price">
          <h1 class="h1-heading">$0.00</h1>
        </div>

        <div class="error-section-right">
          <div>Jul 2023</div>
        </div>
      </div>
    </div>

    <div class="content-div">
      <div class="google-Gsuite">
        <div class="right-divs">
          <img src="../assets/right-arrow (1).png" alt="" />
          <h4>Dental & Vision Expense</h4>
        </div>

        <div class="left-img">
          <img src="../assets/i.png" alt="" />
          <img src="../assets/dots.png" alt="" />
        </div>
      </div>

      <!-- // error section -->

      <div class="error-sections">
        <div class="price">
          <h1 class="h1-heading">$680.00</h1>
        </div>

        <div class="error-section-right">
          <div>Jul 2023</div>
        </div>
      </div>
    </div>

    <div class="content-div">
      <div class="google-Gsuite">
        <div class="right-divs">
          <img src="../assets/right-arrow (1).png" alt="" />
          <h4>Health Insurance Expense</h4>
        </div>

        <div class="left-img">
          <img src="../assets/i.png" alt="" />
          <img src="../assets/dots.png" alt="" />
        </div>
      </div>

      <!-- // error section -->

      <div class="error-sections">
        <div class="price">
          <h1 class="h1-heading">$3,500.00</h1>
        </div>

        <div class="error-section-right">
          <div>Jul 2023</div>
        </div>
      </div>
    </div>

    <div class="content-div">
      <div class="google-Gsuite">
        <div class="right-divs">
          <img src="../assets/right-arrow (1).png" alt="" />
          <h4>Advertising Budget</h4>
        </div>

        <div class="left-img">
          <img src="../assets/i.png" alt="" />
          <img src="../assets/dots.png" alt="" />
        </div>
      </div>

      <!-- // error section -->

      <div class="error-sections">
        <div class="price">
          <h1 class="h1-heading">$11,652.00</h1>
        </div>

        <div class="error-section-right">
          <div>Jul 2023</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import inputs from "./HelloWorld.vue";
</script>

<style>
.Ui-container {
  width: 95%;
  max-height: 100%;
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.google-Gsuite {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  background-color: #e9e4e498;
  height: 40px;
}

.right-div {
  padding-left: 10px;
  gap: 10px;
}
.google-Gsuite div {
  display: flex;
  align-items: center;
  gap: 5px;
}

.left-img img {
  width: 25px;
  margin-right: 5px;
}

.left-img {
  padding-right: 10px;
  gap: 10px;
}

h4 {
  color: #5a5959;
}

/* error section */

.error-section {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  background-color: #f7f4f4;
  height: 60px;
}

.error-section div {
  display: flex;

  align-items: center;
  padding-left: 10px;
  gap: 15px;
}

.error-section .price h1 {
  margin-left: 16px;
  font-weight: 300;
}

.error-section-right div {
  padding: 4px 4px 4px 4px;
  border-radius: 5px;
  margin-right: 30px;
  background-color: #ebe4e4;
}

/* input section  */
.input {
  width: 100%;
  margin: auto;
}

/* Add time  */

.add-time-containe {
  width: 97%;
  height: 40px;
  border-bottom: 1px solid rgb(196, 194, 194);
  display: flex;
  align-items: center;
  padding: 10px;
}
.span-div span {
  display: flex;
  align-items: center;
  color: blue;
}

.span-div {
  margin-left: 10px;
}

.span-div img {
  width: 16px;
  margin-right: 5px;
}
.right-divs{

    margin-left: 15px;
}
.right-divs img{

    width: 18px;

}

.content-div {
  width: 100%;
  margin-top: 30px;
}

.h1-heading {
  width: 15px;
  margin-left: 16px;
  font-weight: 300;
}

.error-sections {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: space-between;
  background-color: #f7f4f4;
  height: 60px;
}

.error-sections div {
  display: flex;

  align-items: center;
  padding-left: 10px;
  gap: 15px;
}

.error-sections-right div {
  padding: 4px 4px 4px 4px;
  border-radius: 5px;
  margin-right: 30px;
  background-color: #ebe4e4;
}
</style>
