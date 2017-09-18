<template>
  <div id="app">
  <div class="content">
    <section class="head">
      <h1>Vue drag Verify</h1>
      <h3>This is a vue component, which is sliding to unlock some functionalities, such as login or sign up. This is used to protect your web app from attack.</h3>

    </section>
    
    <section class="demo-section">
    <div class="column">
      <drag-verify :width="400" :handler-icon="handlerIcon" :success-icon="successIcon"></drag-verify>
    </div>
      
      <div class="column">
           <table>
      <tr>
          <td>width</td>
          <td>
            <input type="text">
          </td>
        </tr>
        <tr>
          <td>height</td>
          <td>
            <input type="text">
          </td>
        </tr>
        <tr>
          <td>text</td>
          <td>
           <input type="text">
          </td>
        </tr>
         <tr>
          <td>successText</td>
          <td>
           <input type="text">
          </td>
        </tr>
        <tr>
          <td>background</td>
          <td>
           <input type="color" >
          </td>
        </tr>
        <tr>
          <td>progressBarBg</td>
          <td>
           <input type="color" >
          </td>
        </tr>
        <tr>
          <td>completedBg</td>
          <td>
           <input type="color" >
          </td>
        </tr>
         <tr>
          <td>handlerWidth</td>
          <td>
            <input type="text">
          </td>
        </tr>

      </table>
      </div>
    </section>
    
  </div>
    
    
  </div>
</template>

<script>
import dragVerify from './dragVerify.vue'
export default {
  name: 'app',
  components:{
    dragVerify
  },
  data () {
    return {
      handlerIcon:'fa fa-angle-double-right',
      successIcon:'fa fa-check'
    }
  }
}
</script>

<style>
body{
   font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color:#fff;
  background: #323232;
}
#app {
 
  
}
.content{
  margin: 60px auto 0;
  width:1000px;
  
}
.head{
  margin-bottom: 50px;
}
.column{
  float: left;
  width:50%;
}
table input{
    font: inherit;
    margin-bottom: 10px;
    margin: 0;
    outline: none;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
    float:left;
}

table { display: block; width: 100%; overflow: auto; word-break: normal; word-break: keep-all; -webkit-overflow-scrolling: touch; }
table th { font-weight: bold; }
table th, table td { padding: 0.5rem 1rem;  }
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
