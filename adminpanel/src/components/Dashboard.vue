<template>
  <v-container fluid class="pa-5">
     <v-layout wrap>
         <v-flex xs12 md8 offset-md2>
             <h1 class="heading blue--text">All of the Blog Posts</h1>
         </v-flex>
           
         <v-flex xs12 md8 offset-md2>
             
             <v-list rounded text three-line class="transparent">
                 <v-list-item-title v-for="(post,index) in posts.slice((page-1)* displayAmount,displayAmount*page)" :key="post._id" class="yellow darken-1 mb-2">
                    <h2 class="blue  white--text font-weight-bold mb-1">{{post.title}}</h2>
                   
                    <v-btn @click="updatePostDialog(post)" color="success">Update</v-btn> <v-btn @click="$emit('removePost',index)" color="error">Remove</v-btn>
                 </v-list-item-title>
             </v-list>
         </v-flex>
    <v-flex xs12 md8 offset-md2>
        <v-pagination v-model="page" :length="Math.ceil(posts.length/displayAmount)" />
    </v-flex>
     </v-layout>
  <v-dialog v-model="updateDialog" persistent max-width="768">
          <v-card>
              <v-card-title class="brown darken-1 white--text headline">
                  Update Old Posts here
              </v-card-title>
              <v-card-text>
                  <v-container fluid grid-list-xl>
                      <v-layout wrap>
                          <v-flex xs-12 md6>
                              <v-text-field label="Title" v-if="currentPost" v-model="currentPost.title" />
                              <v-flex xs12 md6>
                                  <v-text-field label="Author" v-if="currentPost" v-model="currentPost.author"/>
                              </v-flex>
                               <v-flex xs12>
                                   <v-textarea label="Content of the Blog Post" v-if="currentPost" v-model="currentPost.content"/>
                              </v-flex>
                          </v-flex>

                      </v-layout>
                  </v-container>
              </v-card-text>
              <v-card-actions class="orange lighten-1">
                  <v-btn  @click="updateDialog = !updateDialog" color="pink white--text">Close</v-btn>
                  <v-spacer></v-spacer>
                  <v-btn @click="updatePost"  color="green white--text">UpdatePost</v-btn>

              </v-card-actions>
          </v-card>
      </v-dialog>
  </v-container>

</template>

<script>
export default {
    name: 'Dashboard',
    props:['posts'],
    data () {
        return {
            updateDialog: false,
            currentPost: undefined,
            page: 1,
            displayAmount: 5
        }
    },
    methods:{
        updatePostDialog(post){
            this.currentPost =  post;
            this.updateDialog = true;

        },
        updatePost() {
            this.$emit('updatePost',this.currentPost);
            this.updateDialog = false;
        }

    }
}
</script>

<style>

</style>