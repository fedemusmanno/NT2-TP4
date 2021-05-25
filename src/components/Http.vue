<template>
  
  <section class="src-components-http">
    <div class="jumbotron">
    
      <h2>Solicitud MockApi</h2>
      <hr>
      <br>
      <button class="btn btn-danger my-3 mr-3" @click="getPostsCb()">Pedir XMLHttpRequest</button>
      <button class="btn btn-warning my-3 mr-3" @click="getPostsFetch()">Pedir Fetch</button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsAxios()">Pedir Axios</button>
      <br>
      
      <div v-if="posts.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th v-for="(col,index) in getCols" :key="index">{{col}}</th>
          </tr>
          <tr v-for="(post,index) in posts" :key="index">
            <td v-for="(col,index) in getCols" :key="index">{{post[col]}}</td>
          </tr>
        </table>
      </div>
    </div>
  </section>
  
</template>

<script lang="js">
  export default {
    name: 'Http',
    props: [],
    mounted(){

    },
    data(){
      return {
        url : 'https://60ad4ee580a61f0017330b53.mockapi.io/usuarios',
        posts : []
      }
    },
    methods:{
      getPostsCb(){
        let xhr = new XMLHttpRequest
        xhr.open('get',this.url)
        xhr.addEventListener('load', () => {
          if(xhr.status == 200){
            let respuesta = JSON.parse(xhr.response)
            this.posts = respuesta
          }
          else{
            console.error(`Error en GET -> status: ${xhr.status}`)
          }
        })
        xhr.addEventListener('error',e => {
            console.error(`Error XMLHttpRequest ->`, e)
        })
        xhr.send() 
      },
      getPostsFetch(){
        fetch(this.url)
        .then(datos => datos.json())
        .then(respuesta => {
          this.posts = respuesta
        })
        .catch(error => console.error(error))
      },
      getPostsAxios(){
        this.axios(this.url)
        .then(respuesta => {
          this.posts = respuesta.data
        })
        .catch(error => console.error(error))
      }
    },
    computed: {
      getCols(){
        return Object.keys(this.posts[0])
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="css">

  .jumbotron{
    background-color: rgba(168, 27, 27, 0.836);
    color: white;
  }

  hr{
    background-color: #eee;
  }

</style>
