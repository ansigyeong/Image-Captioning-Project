<template>
  <div class="bg1">
    <Navbar/>
    <div style="min-height: 100%;">
      <div class="bin"></div>
      
      <div class="container">
        <div style="text-align: center;"><h1>🔉 Notice 🔉</h1></div>
        <br>
        <br>
        <div style="text-align: right;"><v-btn @click='goCreateNotice'>공지사항 작성(관리자)</v-btn></div>
        <br>
        <br>
        <div class="content-back">
          <div style="text-align: center;"> 
            <v-row>
                <v-col cols="3">번호</v-col>
                <v-col cols="6">제목</v-col>
                <v-col cols="3">작성 일자</v-col>
            </v-row>
            <hr>
            <div v-for="notice in calData" :key="`notice_${notice.id}`">
                <v-row>
                    <v-col cols="3">{{ notice.id }}</v-col>
                    <v-col cols="6" @click="goDetail(notice.id)">{{ notice.title }}</v-col>
                    <v-col cols="3">{{ notice.created_at | moment('YYYY-MM-DD') }}</v-col>
                </v-row>
            </div>
          </div>
        </div>
      </div>
      <div class="text-center">
        <v-pagination
          v-model="curPageNum"
          :length="numOfPages"
          circle
        ></v-pagination>
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
  name: 'noticeList',
  components: {
        Navbar,
        Footer,
  },
  data() {
    return {
      notices: [],
      dataPerPage: 5,
      curPageNum: 1,
    };
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      http.get("/community/notice/")
        .then(res => this.notices = res.data)
        .catch(err => console.error(err))
    },
    goCreateNotice() {
      const config = {
          headers: {
              'Authorization': `Token ${this.$cookies.get('auth-token')}`
          }
      }
      http.post('/community/notice/check/', null, config)
      .then(res => {
        if (res.data == '통과') {
          this.$router.push('/notice/createnotice/')
        } else {
          alert('운영자만 공지사항을 작성할 수 있습니다')
        }
      })
    },
    goDetail(id) {
      this.$router.push('/notice/noticedetail/' + id)
    }
  },
  computed: {
    startOffset() {
      return ((this.curPageNum - 1) * this.dataPerPage);
    },
    endOffset() {
      return (this.startOffset + this.dataPerPage);
    },
    numOfPages() {
      return Math.ceil(this.notices.length / this.dataPerPage);
    },
    calData() {
      return this.notices.slice(this.startOffset, this.endOffset);
    },
  },
}
</script>

<style scoped>
  .bin{
    height: 70px;
  }
  @media(max-width: 480px){
    h1{
      font-size: 30px;
    }
  }
  .content-back {
    background-color: rgb(255, 255, 255, 0.9);
    border-radius: 1rem;
  }
</style>
<style>
  .theme--light.v-pagination .v-pagination__item--active {
    color: black;
  }
</style>