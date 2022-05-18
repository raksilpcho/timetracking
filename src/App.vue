<template>
  <nav>
     <el-icon :size="size" :color="color">
      <Edit />
    </el-icon>
    Vue Time Tracker 
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>  
  </nav>
  <router-view />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 20px;
  text-align: left;
  background-color: #e8e8e8;
}

nav a {
  font-weight: bold;
  color: #808080;
  display: inline-block;
  padding: 5px 10px;

 
}

nav a.router-link-exact-active {
  color: black;
}
</style>
