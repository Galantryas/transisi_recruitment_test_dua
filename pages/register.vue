<template>
  <v-container class="container">
      <div class="box">
          <v-card>
          <v-card-title>
            <span class="headline">Register</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    v-model="email"
                    label="Email"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="password"
                    label="Password"
                    type="password"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <div class="ml-4">
            <a
              style="color:black; font-size:80%; cursor:context-menu;"
              color="black"
            >
              Already have an account? 
            </a>
            <router-link :to="{ path: 'login'}">
              <a
                style="text-decoration:underline; font-size:80%;"
                color="blue darken-1"
              >
                Login
              </a>
            </router-link>
          </div>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="register"
            >
              CONFIRM
            </v-btn>
          </v-card-actions>
        </v-card>
        <v-snackbar
          v-model="snackbar"
          :timeout="timeout"
        >
          {{ text }}

          <template v-slot:action="{ attrs }">
            <v-btn
              color="blue"
              text
              v-bind="attrs"
              @click="snackbar = false"
            >
              Close
            </v-btn>
          </template>
        </v-snackbar>
      </div>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      email: null,
      password: null,
      snackbar: false,
      text: null,
      timeout: 2000,
    }
  },
  mounted(){
  },
  methods: {
    async register(){
      try {
        const bodyRequest = {
          email: this.email,
          password: this.password,
        };
          const { token } = await this.$axios.$post('https://reqres.in/api/register', bodyRequest);
          const registerData = token || [];
          this.$store.commit('changeToken', registerData);
          this.text = "Register Success";
          this.snackbar=true;
          this.$router.push('/');
          return token;
        } catch ({response}) {
          this.text=response.data.error;
          this.snackbar=true;
          console.log(response.data.error);
        } finally {
      }
    },
  }
}
</script>

<style lang="scss" scoped>
  .container{
    max-width: 100vw;
    padding:0px;
  }
  .box{
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-image: url('../static/detail.gif');
    background-size: 100vw 100vh;
  }
</style>
