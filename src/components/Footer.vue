<template>
    <footer>
        <p>Copyright &copy; 2021</p>
        <router-link v-show="!aboutPage" to="/about">About</router-link>
    </footer>
</template>

<script>
    export default {
        name: 'Footer',
        computed: {
            aboutPage(){
                if(this.$route.path == '/about'){
                    return true
                }else{
                    return false
                }
            }
        }
    }
</script>
<style scoped>
a {
  color: #333;
}
footer {
  margin-top: 30px;
  text-align: center;
}
</style>