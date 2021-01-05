<template>
  <v-container class="container">
      <div class="box">
          <img style="border-radius:100%;width:20%;" :src=userData.avatar alt="">
          <h1>Hi! My Name {{userData.first_name}} {{userData.last_name}}</h1>
          <a>Contact Me :</a>
          <v-icon @click="openGmail" color="white" class="mt-2">mdi-gmail</v-icon>
          <router-link :to="{ path: '/'}">
          <div class="cornerup">
            <v-icon color="black">mdi-keyboard-backspace</v-icon>
          </div>
          </router-link>
      </div>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      userData: [],
    }
  },
  beforeMount(){
    if(this.$store.state.token==null){
      this.$router.push('/login');
    }
  },
  mounted(){
    this.getUserData();
    console.log(this.$route.query.userId);
  },
  methods: {
    async getUserData(){
      try {
          const { data } = await this.$axios.$get('https://reqres.in/api/users/'+this.$route.query.userId, {
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
            },
          });
          const userData = data || [];
          this.userData = userData;
          console.log(this.userData);
          return data;
        } catch ({response}) {
          console.log(response.data.error);
        }
    },
    openGmail(){
      window.open("https://mail.google.com/mail/?view=cm&fs=1&to="+this.userData.email, '_blank');
    }
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
      color: white;
      background-image: url('../static/details.gif');
      background-size: cover;
      background-repeat: no-repeat;
  }
  a{
    color:white;
    cursor: context-menu;
  }
  .cornerup{
    cursor: pointer;
    border-radius:50%;
    background-color: rgb(255, 255, 255);
    position: absolute;
    top: 2.8%;
    left: 2.8%;
  }
</style>
