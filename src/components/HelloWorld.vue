<template>
  <div class='hello'>
    <section class="hero is-info ">
      <div class="container">
        <div class="colums">
          <div class="column is-4 is-offset-4">
            <h2>Search</h2><input type="text" name="" value="" class="input is-primary" v-model="search">
          </div>
        </div>
      </div>
    </section>
    <section class='hero is-dark is-fullheight'>
      <div class="container">
        <br>
        <div class="columns">
          <div class="column is-narrow is-4">
            <div class="box hero is-info">
              <h3>ชื่อเรื่อง </h3>
              <input type="text" v-model="newNote.name" name="" value="" class="input" placeholder="title">
              <h3>เนื้อหา </h3>
              <textarea name="name" v-model="newNote.content" rows="8" cols="80" class="textarea" placeholder="content"></textarea>
              <br>
              <button type="button" @click="add()" name="button" class="button is-large is-warning">+ Add</button>
            </div>
          </div>
          <div class="column is-8 box hero is-info">
            <div class="column box" v-for="dat in filteredList">
              <span class="tag is-success is-large" ><b>{{dat.name}}</b></span>
              <span class="tag is-danger is-medium" >ลบ<button class="delete" @click="del(dat)"></button></span>
              <br>
              {{dat.content}} <br>
              <small>{{dat.date}}</small>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyBSCAbd8ve7H2WAgRlkpNc0gly3yN_4XRI',
  authDomain: 'chat-kewin.firebaseapp.com',
  databaseURL: 'https://chat-kewin.firebaseio.com',
  projectId: 'chat-kewin',
  storageBucket: 'chat-kewin.appspot.com',
  messagingSenderId: '385123468341'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let dataRef = db.ref('data')
export default {
  name: 'HelloWorld',
  firebase: {
    data: dataRef
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newNote: {
        name: '',
        content: '',
        date: this.$moment(Date.now()).format('DD-MM-YYYY'),
        dat: ''
      },
      search: ''
    }
  },
  methods: {
    add () {
      if (this.newNote.name === '' || this.newNote.content === '') {
        this.$swal(
          'กรุณาใส่ข้อมูลให้ครบ!',
          'กรุณาตรวจสอบข้อมูลของคุณให้ครบถ้วนด้วยค่ะ!',
          'error'
        )
      } else {
        dataRef.push(this.newNote)
        this.newNote.name = ''
        this.newNote.content = ''
        this.$swal(
          'Success',
          'เพิ่มข้อมูลสำเร็จ',
          'success'
        )
      }
    },
    del (dat) {
      dataRef.child(dat['.key']).remove()
      this.$swal(
        'Deleted',
        'ลบข้อมูลสำเร็จ',
        'error'
      )
    }
  },
  computed: {
    filteredList () {
      var a = this.data.filter(post => { return post.name.includes(this.search) || post.content.includes(this.search) })
      return a
    }
  }
}
</script>

<style scoped>
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
#navbar-primary .navbar-nav {
//  background: #ededed;
  width: 100%;
  text-align: center;
  > li {
    display: inline-block;
    float: none;
    > a {
      padding-left: 30px;
      padding-right: 30px;
    }
  }
}
</style>
