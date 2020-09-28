<template>
  <div id="contact">
    <b-container>
      <b-row>
        <div
          v-if="show_contact == true"
          class="col-md-8 ml-auto mt-5 mr-auto border-box"
        >
          <h2 class="text-center">
            Your vision is about to be realized. Let's join forces!
          </h2>

          <div v-if="contact_notice != ''" class="alert alert-danger">
            There was a problem submitting your message. {{ contact_notice }}
          </div>

          <b-form class="contact-form">
            <b-row>
              <div class="col-md-6">
                <b-input-group class="input-group">
                  <div class="input-group-prepend">
                    <span class="input-group-text">
                      <i class="nc-icon nc-single-02"></i>
                    </span>
                  </div>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Name"
                    v-model="contact_name"
                  />
                </b-input-group>
              </div>
              <div class="col-md-6">
                <b-input-group>
                  <div class="input-group-prepend">
                    <span class="input-group-text">
                      <i class="nc-icon nc-email-85"></i>
                    </span>
                  </div>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Email"
                    v-model="contact_email"
                  />
                </b-input-group>
              </div>
            </b-row>
            <br />
            <b-textarea
              class="form-control"
              rows="4"
              placeholder="Tell us your thoughts and feelings..."
              v-model="contact_message"
            ></b-textarea>
            <p class="text-center">
              This is the first step towards building your next great project.
              We'll help you make a lasting impact
            </p>
            <b-row>
              <div class="col-md-4 ml-auto">
                <button
                  @click.prevent="sendContactMessage"
                  class="btn mt-3 btn-lg btn-fill"
                >
                  Send
                </button>
              </div>
            </b-row>
          </b-form>
        </div>

        <div class="" v-else>
          <h3>Message Was Not Successfully Sent,</h3>
          <p>
            We are experiencing some server issue please contact us through our
            hotline below
          </p>
        </div>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import {
  BContainer,
  BRow,
  BInputGroup,
  BFormTextarea,
  BForm,
} from 'bootstrap-vue';
import axios from 'axios';
export default {
  name: 'Contact',
  components: {
    BContainer,
    BRow,
    BInputGroup,
    'b-textarea': BFormTextarea,
    BForm,
  },

  data() {
    return {
      contact_email: '',
      contact_message: '',
      show_contact: true,
      contact_name: '',
      contact_notice: '',
    };
  },

  methods: {
    sendContactMessage() {
      if (this.contact_name.length < 2) {
        this.contact_notice = 'Your name is too short';
      } else if (!this.validEmail(this.contact_email)) {
        this.contact_notice = 'The email address is badly formatted.';
      } else if (this.contact_message.length < 10) {
        this.contact_notice = 'Your message is too short';
      }
      const data = {
        name: this.contact_name,
        email: this.contact_email,
        message: this.contact_message,
      };
      axios
        .post('http://localhost:7000/send', data)
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    validEmail(email) {
      var re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
  },
};
</script>

<style scoped>
.btn {
  background-color: red;
  color: #fff;
  float: right;
}
input,
.form-control {
  background: #f7f7f7;
  outline: none;
}
.btn:hover {
  background-color: #fff;
  color: red;
}
.input-group-prepend > span {
  background-color: #141618;
}
.border-box {
  box-shadow: 0 0 5px rgba(146, 161, 176, 0.15);
  border-radius: 10px;
  padding: 3rem;
}

@media screen and (max-width: 768px) {
  .border-box {
    box-shadow: 0 0 0px;
  }
  .btn {
    float: right;
    height: 3rem;
    width: 5rem;
  }
  .input-group {
    margin-bottom: 1rem;
  }
}
</style>
