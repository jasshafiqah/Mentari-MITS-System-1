<template>
  <div class="bgDef my-auto px-auto">
    <div class="icon-bar" style="bg-color: white;">
      <a href="#" class="fa fa-facebook"></a>
      <a href="#" class="fa fa-twitter"></a>
      <a href="#" class="fa fa-dribbble"></a>
    </div>
    <div class="bgImg">
      <img src="./assets/logo01.png" width="608" height="260" />
    </div>
    <div class="lists">
      <div class="row cancel-marg">
        <div class="margincard">
          <div vertical>
            <b-button-group vertical style="width: 10cm;">
              <b-button class="btn-default" @click="staff">
                <a class="text-btn-default">FIMM</a>
              </b-button>
              <br/>
              <b-button class="btn-default" @click="distributor">
                <a class="text-btn-default">DISTRIBUTOR</a>
              </b-button>
              <br/>
              <b-button class="btn-default" @click="consultant">
                <a class="text-btn-default">CONSULTANT</a>
              </b-button>
              <br/>
              <b-button class="btn-default" @click="others">
                <a class="text-btn-default">OTHERS</a>
              </b-button>
            </b-button-group>
          </div>
          <!-- <va-list fit class="center">

            <div>
              <va-item class="">
                <va-item-section side>

                </va-item-section>
              </va-item>
              <va-item>
                <va-item-section side>
                  <b-button class="btn" href="/auth-consultant/login-consultant">
                    DISTRIBUTOR
                  </b-button>
                </va-item-section>
              </va-item>
              <va-item>
                <va-item-section side>
                  <b-button class="btn" href="">
                    CONSULTANT
                  </b-button>
                </va-item-section>
              </va-item>
              <va-item>
                <va-item-section side>
                  <b-button class="btn" href="">
                    OTHERS
                  </b-button>
                </va-item-section>
              </va-item>
            </div>
          </va-list> -->
        </div>
      </div>
    </div>
    <div class="footer">
      <p><span>&#169; 2021 </span>Federation Of Investment Managers Malaysia [199301017839 (272577-P)]</p>
    </div>
  </div>
</template>
<script>
export default {
  methods: {
    staff () {
      this.$router.push('/auth/login')
    },
    distributor () {
      this.$router.push('/auth-distributor/login-distributor')
    },
    consultant () {
      this.$router.push('/auth-consultant/login-consultant')
    },
    others () {
      this.$router.push('/auth-others/login-others')
    },
  },
}
</script>
<style lang="scss" >
.bgDef{
  min-height: 100vh;
  background-color: black;
  background-image: url('./assets/klcc.jpg');
  background-repeat: no-repeat;
  background-size: cover;
}
.bgImg{
    margin-left: 105px;
    margin-top: 4%;
    position: absolute;
}
.fa:hover {
    opacity: 0.7;
}

.fa-facebook {
  background: #3B5998;
  color: white;
}

.fa-twitter {
  background: #55ACEE;
  color: white;
}

.fa-dribbble {
  background: #ea4c89;
  color: white;
}
.btn-default{
  width:100%;
  padding: 20rem;
  color:transparent;
  width:4cm;
  height:2cm;
  background-color: rgba(255, 255, 255, .15) !important;
  backdrop-filter: blur(3px) !important;
}
.text-btn-default{
  color:white;
  font-weight: 600;
  font-size: 14pt;
}
.margincard{
      margin-right: 12%;
      margin-top: 9%;

}
// .va-card{
//   background-color: transparent !important;
// }

.cancel-marg{
  margin-left: none !important;
  margin-right: none !important;
  justify-content: flex-end !important;
}
.item{
    justify-items: center;
    vertical-align: initial;
    writing-mode: vertical-lr;
}
.text-center{
  color: black;
  font-weight: 400;
}
.footer {
  color: white;
  position: absolute;
  padding: 20px;
  right: 0;
  left: 0;
  bottom: 0;
  justify-items: center;
  display: inline-block;
  align-self: flex-end;
  margin-left: 35%;
  font-weight: 800;
}
.social-icon{
  display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;

    -ms-flex-align: center;
    -webkit-align-items: center;
    -webkit-box-align: center;

    align-items: center;
}
* {
  box-sizing: border-box;
}
.fluid-container.footer > *:last-child {
    margin-bottom: 0px;
    color: #fff;
}
.icon-bar {
  position: fixed;
  top: 50%;
  -webkit-transform: translateY(-50%);
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
}

.icon-bar a {
  display: block;
  text-align: center;
  padding: 16px;
  transition: all 0.3s ease;
  color: white;
  font-size: 20px;
}

.icon-bar a:hover {
  background-color: rgb(190, 178, 178);
}
.btn-group button {
  background-color: #4CAF50; /* Green background */
  border: 1px solid green; /* Green border */
  color: white; /* White text */
  padding: 10px 24px; /* Some padding */
  cursor: pointer; /* Pointer/hand icon */
  width: 50%; /* Set a width if needed */
  display: block; /* Make the buttons appear below each other */
}

.btn-group button:not(:last-child) {
  border-bottom: none; /* Prevent double borders */
}

/* Add a background color on hover */
.btn-group button:hover {
  background-color: #3e8e41;
}
</style>
