<template>
  <center>
  <div class="hello" style="padding-top:100px; width:500px;">
    <div>
      <img src="https://78.media.tumblr.com/775de8eb8487af171fa0cf9ad1620f23/tumblr_o03a0qc0Ne1v05zj7o1_500.gif" />
      <h1>Decentralized Diary</h1>
      <p>A decentralized diary, just for you, built on <a href="https://blockstack.org" target="_blank">Blockstack</a></p>
      <button class="btn btn-default" @click.prevent="signIn">Sign In With Blockstack</button>

      <div style="padding-top:70px; text-align:left;">
        Why decentralization?
        <ol>
          <li>You own and control all of your data. It is not stored in a central database.</li>
          <li>Besides Blockstock (and maybe your data storage preference) this app uses no thrid-party services.</li>
          <li>Your data can only be accessed using your private key, which is tied to your device.</li>
          <li>All communication is encryped.</li>
          <li>For developers, there is very little infrastructure cost when adding many more and concurrent users.</li>
          <li>Decentralization is cool and new!</li>
        </ol>
      </div>
      
      <small class="creds" style="padding-top:100px;">
        Created by <a href="http://twitter.com/theschnaz" target="_blank">Gregory Schnese</a> Source code on <a href="https://github.com/blockstack/blockstack-todos" target="_blank">Github</a>
      </small>

    </div>
  </div>
  </center>
</template>

<script>
export default {
  name: 'landing',
  data () {
    return {
      blockstack: window.blockstack
    }
  },

  methods: {
    signIn () {
      const blockstack = this.blockstack
      blockstack.redirectToSignIn()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
