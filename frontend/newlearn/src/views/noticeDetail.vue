<template>
  <div class="bg1">
    <Navbar/>
    <div style="min-height: 100%;">
      <div class="bin"></div>

      <!-- <div style="text-align: center;"><h1>🔉 Notice 🔉</h1></div>
      <br>
      <br> -->
      <div class="container">
        <div class="content-back">
          <!-- title & etc -->
          <div class="title">
            <h1>{{ this.notice.title }}</h1>
          </div>
          <!-- <div style="text-align: right;">
          </div> -->
          <div style="text-align: right;">
            <v-btn icon style="margin-right: 50px;">{{ this.notice.created_at | moment('YYYY-MM-DD') }}</v-btn>
            <v-btn icon @click="goEdit" style="margin-right: 50px;"><v-icon left>mdi-pencil</v-icon>EDIT</v-btn>
            <v-btn icon @click="goDelete" style="margin-right: 50px;"><v-icon left>mdi-cancel</v-icon>DELETE</v-btn>
          </div>

          <hr>

          <!-- content -->
          <h3><div v-html="this.notice.content" style="margin:20px" class="contentbox"></div></h3>
          <!-- <div class="content">
            <h3>{{ this.suggestion.content }}</h3>
          </div> -->
        </div>
      </div>
    </div>
    <Footer/>
  </div>
</template>

<script>
import http from '../util/http-common.js'
import Navbar from "../components/common/Navigation"
import Footer from "../components/common/footer"
import '@/assets/css/background.css'

export default {
  name: 'noticeDetail',
  components: {
        Navbar,
        Footer,
  },
  data() {
    return {
      notice: [],
    }
  },
  created() {
    this.notice_pk = this.$route.params.notice_pk
    this.fetchData()
  },
  methods: {
    fetchData() {
      const config = {
        headers: {
          Authorization: `Token ${this.$cookies.get('auth-token')}`
        }
      }
      http.get("/community/notice/" + this.notice_pk, config)
      .then((res) => {
        this.notice = res.data
      })
      .catch(err => console.log(err))
    },
    goDelete() {
      const config = {
        headers: {
          Authorization: `Token ${this.$cookies.get('auth-token')}`
        }
      }
      http.delete(`/community/notice/` + this.notice_pk + `/delete/`, config)
      .then(() => {
        alert('성공적으로 삭제되었습니다.')
        this.$router.go(-1)
        // this.$router.push({ name: 'vocList' })
      })
      .catch((err) => {
        console.log(err)
        alert('성공적으로 삭제되었습니다.') // 여기서 삭제됨, 에러 해결하기
        this.$router.go(-1)
        // this.$router.push({name: 'vocList'})
      })
    },
    goEdit() {
      this.$router.push('/notice/editnotice/' + this.notice_pk)
    }
  }
}
</script>

<style scoped>
  .bin{
    height: 70px;
  }
  .content-back {
    background-color: rgb(255, 255, 255, 0.9);
    border-radius: 1rem;
    min-height: 300px;
  }
  .title {
    margin: 20px;
  }
  .content {
    margin: 20px;
  }
  .v-btn {
    margin: 5px;
  }  
  @media(max-width: 480px){
    h1{
        font-size: 30px;
    }
  }
</style>