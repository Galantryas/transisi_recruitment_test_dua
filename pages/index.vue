<template>
  <v-container>
    <v-toolbar-title style="padding: 3vh 0 0 2vw">User List</v-toolbar-title>
      <v-btn class="cornerup"
        color="primary"
        dark
        v-on="on"
      >
        Add User
      </v-btn>
    <v-divider
      class="mx-4"
      inset
      vertical
    ></v-divider>
    <v-spacer></v-spacer>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">
              Name
            </th>
            <th class="text-left">
              Email
            </th>
            <th class="text-left">
              Avatar
            </th>
            <th class="text-left">
              Action
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="item in userList"
            :key="item.id"
          >
            <td>
              <router-link :to="{ path: 'detail', query: { userId: item.id }}" >
                <v-btn color="white"><a>{{ item.first_name }} {{item.last_name}}</a></v-btn>
              </router-link>
            </td>
            <td>{{ item.email }}</td>
            <td><img :src=item.avatar alt=""></td>
            <td>
              <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
              >
                mdi-pencil
              </v-icon>
              <v-icon
                small
                @click="deleteItem(item)"
              >
                mdi-delete
              </v-icon>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-container>
</template>

<script>

export default {
  components: {
  },
  data () {
    return {
      userList: [],
    }
  },
  mounted(){
    this.getUserList();
  },
  methods: {
    async getUserList(){
      try {
          const { data } = await this.$axios.$get('https://reqres.in/api/users', {
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
            },
          });
          const userList = data || [];
          this.userList = userList;
          console.log(this.userList);
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
  .cornerup{
    position: absolute;
    top: 2.2%;
    right: 1%;
  }
  a{
    text-decoration: none;
    color: black;
  }
</style>
