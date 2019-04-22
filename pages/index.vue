<template>
  <div class="d-flex pt-4 h-100">
    <div class="w-100">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
              <a href="#">xCurrGPGames</a><br>
              <input v-on:keyup.enter="searchUser" ref="el" v-model="handle" type="text" placeholder="@PSNusername" autofous><br>
          </div>
        </div>
        <div class="row pl-3 pr-3">
          <hr>
        </div>
      </div>
      <div class="container">
        <div class="row">
          <div class="col-md-12 text-center pt-2">
            <div class="text-main">{{ gamesPlayed }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data: function () {
    return {
      APIconfig: {
        headers: { 'X-API-Key': 'bbb4ecfaddd943faae70ea2dcb84254f' }
      },
      handle: '',
      gamesPlayed: 0,
      memID: null,
      memIDs: null
    }
  },
  methods: {
    searchUser () {
      if (this.handle !== '') {
        this.$refs.el.blur()
        this.getIDByUsername()
      }
    },
    getIDByUsername: function () {
      let query = this.handle
      let url = 'https://www.bungie.net/platform/Destiny2/SearchDestinyPlayer/-1/' + query

      console.log(url)

      this.$axios(url, this.APIconfig).then(response => {
        this.memID = response.data.Response[0].membershipId
        this.memIDs = response.data.Response.map(memAccount => [memAccount.membershipId, memAccount.membershipType])

        this.getGPStats()
      })
    },
    getGPStats: function () {
      let url = 'https://www.bungie.net/Platform/Destiny2/2/Account/' + this.memID + '/Character/0/Stats/?modes=63'
      
      this.$axios(url, this.APIconfig).then(response => {
        this.gamesPlayed = response.data.Response.pvecomp_gambit.allTime.activitiesEntered.basic.value

        console.log(response)
      })
    }
  }
}
</script>

<style>
html, body {
  background: url('~assets/infamous_bg.png') no-repeat center center fixed!important;
  background-color: #183743!important;
  background-size: cover!important;
  color: #FFF!important;
}

a, a:hover {
  color: #FFF;
}

input[type=text] {
  background: none;
  color: #FFF;
  border: none;
  outline: none;
}

.text-main {  
  /* font-size: 34rem; */
  font-weight: 700;
  line-height: 72%;
}

hr {
  width: 100%;
  border-top: 1px solid #FFF;
}

@media (min-width: 1200px) {
  .text-main {  
    font-size: 30rem;
  }
}

@media (max-width: 1200px) {
  .text-main {  
    font-size: 30rem;
  }
}

@media (max-width: 992px) {
  .text-main {  
    font-size: 22rem;
  }
}

@media (max-width: 768px) {
  .text-main {  
    font-size: 16rem;
  }
}

@media (max-width: 576px) {
  .text-main {  
    font-size: 14rem;
  }
}

@media (max-width: 472px) {
  .text-main {  
    font-size: 11rem;
  }
}

/* 
@media (min-width: 100px) {
  .text-main {  
    font-size: 34rem;
  }
} */
</style>
