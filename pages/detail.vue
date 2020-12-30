<template>
  <v-container class="container">
      <div class="box">
          <img style="border-radius:100%;margin-top:7vh;width:30%;" :src=userData.avatar alt="">
          <h1>{{userData.first_name}} {{userData.last_name}}</h1>
          <h4>{{userData.email}}</h4>
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
        } catch (response) {
          console.log(response);
        }
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
      display: flex;
      flex-direction: column;
      align-items: center;
  }
</style>
