<template>
  <ul class="nav navbar-nav">
    <li v-for="nav in navList" :key="nav.id"><a class="page-scroll" :href="nav.url">{{nav.name}}</a></li>
    <li><a class="page-scroll" href="/#section-contact">聯系我們</a></li>
  </ul>
</template>

<script>
  import {getNavbar} from '../../api/api'
  export default {
    name: 'NavbarSite',
    data () {
      return {
        navList: []
      }
    },
    computed: {
      // navList: [],
      user () {
        return {
          username: localStorage.getItem('username')
        }
      }
    },
    methods: {
      getNav () {
        var navListData = JSON.parse(sessionStorage.getItem('navlist')) || []
        if (navListData.length === 0) {
          getNavbar({}).then((response) => {
            console.log(response.data)
            this.navList = response.data
            sessionStorage.setItem('navlist', JSON.stringify(response.data))
          })
            .catch((error) => {
              console.log(error.data)
            })
        } else {
          this.navList = navListData
        }
      },
    },
    created () {
      // sessionStorage.clear()
      this.getNav()
    }
  }
</script>

<style scoped>
  /*.navbar{*/
  /*  !*display: flex;*!*/
  /*  !*flex-direction: row;*!*/
  /*  !*border: 2px solid #eeeeee;*!*/
  /*  border-radius: 10px;*/
  /*  width: 36%;*/
  /*  !*height: 60px;*!*/
  /*  margin: 0 auto;*/
  /*  position: absolute;*/
  /*  right: 3%;*/
  /*  float: right;*/
  /*}*/
  .navbar ul{
    padding: 0;
  }
  .navbar li{
    float: left;
    /*float: right;*/
    /*text-align: right;*/
    list-style: none;
    padding: 2px;
    border: 2px solid #cccccc;
    border-radius: 5px;
    margin-right: 5px;
  }
  a {
    background-color: #cccccc;
    text-decoration: none;
    color: #555;
  }

  a:hover {
    color: #FF7E60;
    cursor: pointer;
    transition: all 0.25s ease 0s;
    -moz-transition: all 0.25s ease 0s;
    -webkit-transition: all 0.25s ease 0s;
    -o-transition: all 0.25s ease 0s;
    background-color: transparent;
  }

  a:focus {
    outline: none;
    outline-offset: 0;

  }

  a:link,
  a:visited {
    text-decoration: none;
  }
</style>
