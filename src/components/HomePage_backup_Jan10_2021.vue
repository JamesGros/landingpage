<template>
<div class="container-fluid m-0 p-0">

  <!-- <div id="home-page" class="full-height p-4">
<div class="container">
  <div class="row">

  </div>
</div>
  </div> -->

  <div id="home-page" class="full-height p-4">
        <div class="container pt-3">

          <div class="row pb-5">
            <div class="col-lg-6 pt-4 my-auto">
              <div class="text-center text-light mb-3 d-block d-lg-none">
                <h1 class="display-3">{{ title }}</h1>
                <h3>Lift With Barbell Notebook</h3>
              </div>
              <img id="iphone-img" src="../assets/bn_screenshot.png" class="img-fluid">
            </div>

            <div class="col-lg-6 pt-4 my-auto">
              <div class="text-center text-light mb-3 d-none d-lg-block">
                <h1 class="display-3">{{ title }}</h1>
                <h3>Lift With Barbell Notebook</h3>
              </div>

              <div class="border p-3 p-md-5 bg-white rounded shadow">
                <h2>Coming Soon</h2>
                <form @submit.prevent="addEmail(email)">
                  <div class="form-group">
                    <label for="emailSignup">Reserve your account now, we'll contact you when it's live</label>
                    <input v-model="email"
                           type="email"
                           id="emailSignup"
                           placeholder="Enter Email"
                           class="form-control"
                    >
                    <small id="emailHelp" class="form-text text-muted">We'll never share your email address</small>
                    <button type="submit" class="btn btn-success mt-3">Join Waiting List</button>
                    <div class="mt-4">
                      <p class="m-0">{{ message }}</p>
                    </div>
                  </div>
                </form>
              </div>

              <div id="available" class="p-2">
                <h4 class="text-center mb-2 mt-3 text-light">
                  Available On
                </h4>
                <div class="row">
                  <div class="col my-auto">
                    <img src="../assets/download_apple.png" class="float-right img-fluid app_store_img">
                  </div>
                  <div class="col my-auto">
                    <img src="../assets/download_google.png" class="float-left img-fluid app_store_img">
                  </div>
                </div>
              </div>

            </div>
          </div>

        </div>
      </div>



      <div id="about" class="bg-light p-3 p-md-5">
        <div class="row">
          <div class="col-lg">
            <!-- Icon made by Freepik perfect from www.flaticon.com -->
            <img src="../assets/weightrack.png" height="320" class="m-4">
            <h5 class="ml-md-4 mr-md-4">
              Manage your weight rack inventory.
              We make it easy to load your barbell using your weight rack,
              giving you more time to focus on your lifts!
            </h5>
          </div>
          <div class="col-lg">
            <!-- Icon made by Freepik perfect from www.flaticon.com -->
            <img src="../assets/new_wendler_cycle.png" height="320" class="m-4">
            <h5 class="ml-md-4 mr-md-4">
              Use the app to track your Wendler workout sessions.
              Know exactly what cycle 5/5/5, 3/3/3, or 5/3/1 to continue in the next session.
            </h5>
          </div>
          <div class="col-lg">
            <!-- Icon made by Freepik perfect from www.flaticon.com -->
            <img src="../assets/lbv_wendler.png" height="320" class="m-4">
            <h5 class="ml-md-4 mr-md-4">
              During your workout session, have the peace of mind that plates
              loaded on the barbell are accurate. Never worry about which plates to load again!
            </h5>
          </div>
        </div>
      </div>

      <div id="contact" class="p-4">
        <div class="row justify-content-center mt-3 mb-3">

          <div v-if="show_contact == true" class="col-lg-4">
            <h2>Have Any Questions?</h2>
            <p>Contact us by filling out the information below</p>

            <div v-if="contact_notice != ''" class="alert alert-warning">
              There was a problem submitting your message. {{contact_notice}}
            </div>

            <form @submit.prevent="sendContactMessage()">
              <div class="form-group text-left ">
                <input v-model="contact_email"
                       type="email"
                       class="form-control"
                       placeholder="Enter Your Email"
                       >
                <textarea v-model="contact_message"
                          class="form-control mt-3"
                          placeholder="Enter Your Message"
                          rows="5"
                ></textarea>
              </div>
              <button type="submit" class="btn btn-primary">Send Message</button>
            </form>
          </div>

          <div v-else>
            <h3>Message Sent Successfully!</h3>
            <p>Thank you for contacting us, we'll get back to you as soon as we can.</p>
          </div>
        </div>

      </div>

      <div id="footer" class=" p-4">
        <footer class="text-light p-4">
          <!-- <a href="https://www.youtube.com/1manstartup" target="_blank" class="text-warning">Build This App</a> | -->
          <a href="#" class="text-warning">Privacy Policy</a>
          <br>
          <small>&copy; 2021, BarbellNotebook.app</small>
        </footer>
      </div>
</div>
</template>

<script>
import { Auth } from '@/firebase/auth.js'

export default {

  data () {
    return {
          title: 'Barbell Notebook',
          email: '',
          message: '',
          show_contact: true,
          contact_email: '',
          contact_message: '',
          contact_notice: '',
        }
  },

  methods: {
    async addEmail(email) {
      // console.log("INSIDE addEmail()")
      var noticeMessage = "🎉 Your account has been reserved 🎉"
      await Auth.createUserWithEmailAndPassword(email, this.randomPassword(20)).catch(function(error) {
        if (error.code != "auth/email-already-in-use") {
          noticeMessage = error.message;
        }
      });
      this.message = noticeMessage;
      this.email = '';
    },
    randomPassword(length) {
      var chars = "abcdefghijklmnopqrstuvwxyz!@#$%^&*()-+<>ABCDEFGHIJKLMNOP1234567890";
      var password = "";
      for (var x = 0; x < length; x++) {
        // console.log(Math.floor(Math.random() * chars.length))
        var i = Math.floor(Math.random() * chars.length);
        password += chars.charAt(i);
      }
      // console.log(password)
      return password;
    },
    sendContactMessage() {
      if (!this.validEmail(this.contact_email)) {
        this.contact_notice = 'The email address is badly formatted.';
      } else if (this.contact_message.length < 10) {
        this.contact_notice = "Your message is too short";
      } else {
        const url = `https://us-central1-ios-barbell-notebook.cloudfunctions.net/sendEmail?email_from=${this.contact_email}&message=${this.contact_message}`
        const requestOptions = {
          method: "GET",
          headers: { "Content-Type": "application/json" }
        };
        fetch(url, requestOptions);
        this.show_contact = false;
      }
    },
    validEmail(email) {
      var re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#home-page {
  background-color: #92D2F2;
  background: url('../assets/bg_plates.jpg') no-repeat center center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

#about {
  min-height: 40vh;
}

#iphone-img {
  max-height: 320vh;
}

#contact {
  background-color: #57AEAF;
}

#footer {
  background-color: #252223;
}
.app_store_img{
  max-height: 80px;
}

</style>
