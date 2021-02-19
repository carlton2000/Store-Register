<template>
<v-app dark id="app">
     
    <v-row
      align="center"
      justify="center"
    >
      <v-col
        class="text-center"
        cols="12"
      >
        <h1 class="display-1 font-weight-thin mb-4">
         Storing Data
        </h1>
        <h2 class="subheading">
          Build your application today!
        </h2>
    <br/>
    <v-spacer></v-spacer>
    <v-btn to="/" exact>
      <span class="mr-3">Logout</span>
      <v-icon>mdi-door</v-icon>
    </v-btn>
      </v-col>
    </v-row>
    

    <!-- <v-card class="mx-auto" max-width="944">
      <v-text-field
        class="search"
        v-model="search"
        append-icon="mdi-magnify"
        label="Search Blog"
        single-line
        hide-details
      ></v-text-field>
    </v-card> -->
    <!-- <v-card
      v-for="blog in filteredBlogs"
      :key="blog"
      class="mx-auto"
      max-width="944"
    > -->
      <!-- <v-img
        src="https://www.start-business-online.com/images/article_manager_uploads/blog.jpg"
        height="150px"
      ></v-img> -->

</v-app>
</template>

<script>
export default {
  data() {
    return {
      alert: true,
      blogs: [],
      blog: {},
      search: ""
    };
  },
 

  methods: {
    GetBlogs() {
      let URL = "https://guarded-bayou-13878.herokuapp.com/api/blog";
      // console.log("add", this.add)
      let token = window.localStorage.getItem("token");
      fetch(URL, {
        method: "GET",
        mode: "cors",
        headers: {
          "Content-type": "application/json",
          "x-auth-token": token
        }
      })
        .then(response => response.json())
        .then(json => {
          this.blogs = json;
        })
        .catch(err => console.log("err->", err));
    },
    DeleteBlog(_id) {
      // this.contacts.splice(key, 1)
      let URL = `https://guarded-bayou-13878.herokuapp.com/api/blog/${_id}`;
      let token = window.localStorage.getItem("token");

      fetch(URL, {
        method: "DELETE",
        mode: "cors",
        headers: {
          "x-auth-token": token
        }
      })
        .then(response => response.json())
        .then(json => {
          console.log("json->", json);
          this.GetBlogs();
        })
        .catch(err => console.log("err->", err));
    },
    UpdateBlog(id) {
      let URL = `https://guarded-bayou-13878.herokuapp.com/api/blog/${id}`;
      let _data = this.blog;
      let token = window.localStorage.getItem("token");
      fetch(URL, {
        method: "PUT",
        body: JSON.stringify(_data),
        headers: {
          "Content-type": "application/json",
          "x-auth-token": token
        }
      })
        .then(response => response.json())
        .then(json => {
          console.log("json->", json);
          this.blog = {};
          this.edit = false;
          this.GetBlogs();
        })
        .catch(err => console.log("err->", err));
    },
    toggleUpdate(data) {
      this.edit = true;
      this.blog = data;
      console.log(JSON.stringify(this.blog));
    },

    resetForm() {
      this.form = Object.assign({}, this.defaultForm);
      this.$refs.form.reset();
    },
    submit() {
      this.snackbar = true;
      this.resetForm();
    }
  },
  created() {
    this.GetBlogs();
  },
  mounted() {
    this.GetBlogs();
  }
};
</script>
<style scoped>
#app {
  background-image: url("https://cdn.vuetifyjs.com/images/backgrounds/vbanner.jpg");
}
</style>
