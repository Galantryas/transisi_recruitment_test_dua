<template>
  <v-container>
    <v-toolbar-title style="padding: 3vh 0 0 2vw">User List</v-toolbar-title>
    <div class="cornerup">
      <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
          >
            Add User
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="headline">Add User Profile</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="first_name"
                    label="First Name"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="last_name"
                    label="Last Name"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="email"
                    label="Email"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="avatar"
                    label="Avatar"
                    type="file"
                    accept="image/"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="dialog = false"
            >
              Close
            </v-btn>
            <v-btn
              color="blue darken-1"
              text
              @click="addUser"
            >
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
    <v-tooltip bottom>
      <template v-slot:activator="{ on, attrs }">
        <v-icon class="corneruptwo"
          color="red"
          v-bind="attrs"
          v-on="on"
          @click="logout"
        >
          mdi-logout
        </v-icon>
      </template>
      <span>Logout</span>     
    </v-tooltip>
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
              <a>{{ item.first_name }} {{item.last_name}}</a>
              <router-link :to="{ path: 'detail', query: { userId: item.id }}" >
                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-icon
                      small
                      class="ml-2"
                      v-bind="attrs"
                      v-on="on"
                    >
                      mdi-arrow-right-bold-circle-outline
                    </v-icon>
                  </template>
                  <span>User Detail</span>
                </v-tooltip>
              </router-link>
            </td>
            <td>{{ item.email }}</td>
            <td><img :src=item.avatar alt=""></td>
            <td>
              <!-- <router-link :to="{ name: 'update', params: { userId: item.id }}" > -->
                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-icon
                      small
                      class="mr-2"
                      v-bind="attrs"
                      v-on="on"
                      @click="editUser(item.id)"
                    >
                      mdi-pencil
                    </v-icon>
                  </template>
                  <span>Edit</span>
                </v-tooltip>
              <!-- </router-link> -->
              <v-tooltip bottom>
                <template v-slot:activator="{ on, attrs }">
                  <v-icon
                    small
                    v-bind="attrs"
                    v-on="on"
                    @click="deleteUser(item.id)"
                  >
                    mdi-delete
                  </v-icon>
                </template>
                <span>Delete</span>     
              </v-tooltip>
            </td>
          </tr>
        </tbody>
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
            <v-dialog
              v-model="dialogtwo"
              persistent
              max-width="600px"
            >
        <v-card>
          <v-card-title>
            <span class="headline">Edit User Profile</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="first_name"
                    label="First Name"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="last_name"
                    label="Last Name"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="email"
                    label="Email"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    v-model="avatar"
                    label="Avatar"
                    type="file"
                    accept="image/"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="dialogtwo = false"
            >
              Close
            </v-btn>
            <v-btn
              color="blue darken-1"
              text
              @click="editUserProfile"
            >
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
          </template>
        </v-snackbar>
      </template>
    </v-simple-table>
    <div class="box-center">
      <v-btn @click="getUserList(1)">1</v-btn>
      <v-btn @click="getUserList(2)">2</v-btn>
    </div>
  </v-container>
</template>

<script>

export default {
  components: {
  },
  data () {
    return {
      userList: [],
      id: null,
      dialog: false,
      dialogtwo: false,
      first_name: null,
      last_name: null,
      email: null,
      avatar: null,
      snackbar: false,
      text: null,
      timeout: 2000,
      page: 1,
    }
  },
  beforeMount(){
    if(this.$store.state.token==null){
      this.$router.push('/login');
    }
  },
  mounted(){
    this.getUserList(this.page);
  },
  methods: {
    async getUserList(pages){
      try {
          const { data } = await this.$axios.$get('https://reqres.in/api/users?page='+pages);
          const userList = data || [];
          this.userList = userList;
          return data;
        } catch ({response}) {
          console.log(response.data.error);
        }
    },
    async addUser(){
      try {
        const bodyRequest = {
          first_name: this.first_name,
          last_name: this.last_name,
          email: this.email,
          avatar: this.avatar,
        };
          const { data } = await this.$axios.$post('https://reqres.in/api/users', bodyRequest, {
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
            },
          });
          this.text="User added";
          this.snackbar=true;
          return data;
        } catch ({response}) {
          console.log(response.data.error);
          this.text=response.data.error;
          this.snackbar=true;
        } finally {
          this.dialog=false;
          this.first_name= null;
          this.last_name= null;
          this.email= null;
          this.avatar= null;
      }
    },
    editUser(id){
      this.id=id;
      this.dialogtwo=true;
    },
    async editUserProfile(){
      try {
        const bodyRequest = {
          first_name: this.first_name,
          last_name: this.last_name,
          email: this.email,
          avatar: this.avatar,
        };
          const { data } = await this.$axios.$put('https://reqres.in/api/users'+this.id, bodyRequest, {
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8',
            },
          });
          this.text="User has been updated";
          this.snackbar=true;
          return data;
        } catch ({response}) {
          console.log(response.data.error);
          this.text=response.data.error;
          this.snackbar=true;
        } finally {
          this.dialogtwo=false;
          this.first_name= null;
          this.last_name= null;
          this.email= null;
          this.avatar= null;
      }
    },
    async deleteUser(id){
      try {
          const { data } = await this.$axios.$delete('https://reqres.in/api/users/'+ id);
          this.text="User has been deleted";
          this.snackbar=true;
          return data;
        } catch ({response}) {
          console.log(response.data.error);
          this.text=response.data.error;
          this.snackbar=true;
        }
    },
    logout(){
      this.$store.commit('changeToken',null);
      this.$router.push('/login');
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
    right: 5%;
  }
  .corneruptwo{
    position: absolute;
    top: 2.8%;
    right: 1%;
  }
  .box-center{
    display: flex;
    width: 100vw;
    justify-content: center;
  }
  a{
    text-decoration: none;
    color: black;
  }
</style>
