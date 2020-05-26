<template>
  <nav>
      <v-app-bar dark app>
         <v-app-bar-nav-icon  @click="drawer = !drawer"/>
         <v-spacer></v-spacer>
          <v-toolbar-title class="text-uppercase cyan--text">
              AdminPanel for <span class="font-weight-light">Jalal's</span> Brain Blog
          
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
              <v-btn @click="newPostDialog = !newPostDialog" color="lime blue--text">New Post</v-btn>
          </v-toolbar-items>
      </v-app-bar>
      <v-dialog v-model="newPostDialog" persistent max-width="768">
          <v-card>
              <v-card-title class="teal darken-1 white--text headline">
                  Write New Blog Posts here
              </v-card-title>
              <v-card-text>
                  <v-container fluid grid-list-xl>
                      <v-layout wrap>
                          <v-flex xs-12 md6>
                              <v-text-field label="Title" v-model="newPost.title" />
                              <v-flex xs12 md6>
                                  <v-text-field label="Author" v-model="newPost.author"/>
                              </v-flex>
                               <v-flex xs12>
                                   <v-textarea label="Content of the Blog Post" v-model="newPost.content"/>
                              </v-flex>
                          </v-flex>

                      </v-layout>
                  </v-container>
              </v-card-text>
              <v-card-actions class="cyan lighten-1">
                  <v-btn  @click="newPostDialog = !newPostDialog" color="red white--text">Close</v-btn>
                  <v-spacer></v-spacer>
                  <v-btn @click="addPost"  color="pink white--text">Add</v-btn>

              </v-card-actions>
          </v-card>
      </v-dialog>
      <v-navigation-drawer v-model="drawer" class="grey darken-3" app clipped>
          <h2 class="headline yellow--text">Admin Page</h2>
      </v-navigation-drawer>
  </nav>
</template>

<script>
export default {
    name: 'Navbar',
    data() {
        return {
            drawer: false,
            newPostDialog: false,
            newPost: {}

        }
    },
    methods: {
        addPost() {
            let payload = {
                title: this.newPost.title,
                author: this.newPost.author,
                content:this.newPost.content
            }
            fetch("http://localhost:3000/api/post/new",{
                method:"POST",
                headers: {
                    "content-type": "application/json"
                },
                body: JSON.stringify(payload)
            })
            .then(data => {
                return data.json()
            })
            .then(json => {
                this.$emit('newPost', json.result);
                this.newPostDialog = false;
                this.newPost = {};
            })
        }
    }
}
</script>

<style>

</style>