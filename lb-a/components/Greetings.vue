<template>
  <div>
    <!-- echo greeting -->
    <!-- text box for name -->
    <!-- submit button -->
    <div>
      change to show heroku review deploy
    </div>
    <div>
      <input id="username" v-model="greetingform.username" placeholder="name">
    </div>

    <!-- ------------ -->
    <!-- Submit Button -->
    <!-- ------------ -->
    <div>
      <button id="greeting" class="button" @click="onGreeting ()" :disabled="isDisabled">
        Say Hello
      </button>
    </div>
    <h3>
      {{ page.feedback }}
    </h3>
  </div>

</template>
<script>

import { Constants } from './mixins/Constants.js'
import { ApiHandlers } from './mixins/ApiHandlers.js'
//import { TokenHelper } from './mixins/TokenHelper.js'

export default {

  data () {
    return {
      page: {
        title: 'Sign In',
        subtitle: 'Because',
        feedback:'---'
      },
      greetingform: {
        username: '',
        error: ''
      },
      meta: {
        username :{
          prompt:"User Name",
          status:"",
          regexp: Constants.email()
        }
      }
      //adopter_token: this.$store.state.token
    }
  },

  computed: {

    apiHandlers () {
      return new ApiHandlers(this)
    },
    apiHeader () {
      return {
        'Access-Control-Allow-Origin':'*',
        'Content-Type': 'application/json'
      }
      /*
      return {
        'Authorization': 'Bearer ' + process.env.AAD_API_TOKEN,
        'Content-Type': 'application/json'
      }
      */
    },
    apiUrl () {
      // remove just_fail in signup
      return process.env.HAPI_URL + '/greet'
    },
    apiBody () {
      //return JSON.stringify(this.greetingform)
      return this.greetingform
    },
    isDisabled () {
      return !(this.is_username)
    },
    is_username () { // true when not compliant, expects an email
      return (Constants.user_name().test(this.greetingform.username.trim()))
    }
  }, // end of computed
  methods: {
    log (feedback) {
      /* eslint-disable no-console */
      console.log(feedback)
      /* eslint-enable no-console */
    },
    onGreeting () {
      this.apiHandlers.apiSalutation(this.apiUrl, this.apiHeader, this.apiBody['username'])
        .then((response) => {
          if (response.status === 200) {
            this.page.feedback= response.data.salutation;
            //this.feedBack('Go find a drain to adopt!')
            //this.setToken(response.data.token)
            //this.setExpiresAt(new TokenHelper(response.data.token).getExpiration())
          } else {
            //this.feedBack('Whoa, I did not see this comming (%s)!'.replace('%s', response.status))
            //this.log('Whoa, I did not see this comming (%s)!'.replace('%s', response.status))
            /* eslint-disable no-console */
            console.log('Whoa, I did not see this comming (%s)!'.replace('%s', response.status))
            /* eslint-enable no-console */
          }
        })
        .catch((err) => {
          this.page.feedback=err;
          //this.detoken()

          //if (err.message.includes('403')) {
          //  /* eslint-disable no-console */
          //  console.log('Have you signed up?')
          //  /* eslint-enable no-console */
          //} else {
          //  /* eslint-disable no-console */
          //  console.log('Something unexpected happened during sign in (%s)!'.replace('%s', err))
          //  /* eslint-enable no-console */
          //}

        })
    }
  }
}

</script>

<style>
.NuxtLogo {
  animation: 1s appear;
  margin: auto;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
}

.band {
  width: 100%;
}
</style>
