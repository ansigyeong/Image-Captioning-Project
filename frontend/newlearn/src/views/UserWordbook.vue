<template>
    <div class="bg1">
        <Navbar/>
        <div style="min-height: 100%;">
            <div class="bin"></div>

            <h1 style="text-align:center;">📑 My Wordbook 📑</h1>
            <br>
            <div style="text-align: center;">
                <v-card
                width="350"
                height="200"
                v-for="(word) in words" 
                :key="word.pid"
                style="margin-bottom: 20px; margin-right: 20px; display: inline-block; vertical-align: middle;"
                >
                    <v-card-text style="vertical-align: middle;">
                        <!-- <div>Word of the Day</div> -->
                        <p class="word">
                            {{ word.word }}
                        </p>
                        <p>
                            {{ word.phonetic_symbols }}
                        </p>
                        <div class="text--primary">
                            {{ word.mean }}<br>
                            <!-- 예시 -->
                        </div>
                    </v-card-text>
                    
                    <v-btn
                        text
                        color="deep-purple accent-4"
                        style="text-align: center;"
                        @click="delword(word)"
                    >
                        <i class="fas fa-trash"> 내 단어장에서 빼기</i>
                    </v-btn>
                </v-card>
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
    components: {
        Navbar,
        Footer
    },
    data () {
      return {
        words: '',
      }
    },
    created() {
        const config = {
            headers: {
                'Authorization': `Token ${this.$cookies.get('auth-token')}`
            }
        }
        http.post(`/english/userwordlist/`, null, config)
            .then(res => {
                this.words = res.data
            })
    },
    methods: {
        delword(thisword) {
            const data = {
                "word": thisword.word,
            }
            const config = {
                headers: {
                    'Authorization': `Token ${this.$cookies.get('auth-token')}`
                }
            }
            http.post(`/english/deleteuserword/`, data, config)
            .then( () => {
                alert('내 단어장에서 삭제하였습니다.')
                this.$router.go()
            })
            .catch(err => {
                console.log(err)
            })
        }
    }
}
</script>

<style scoped>
    .speak{
        background-image: url(../assets/images/Newyork.jpg) !important;
        background-size : cover;
    }
    .word{
        font-size: 30px;
    }
    .bin{
        height: 70px;
    }
    
    @media(max-width: 480px){
        h1{
            font-size: 30px;
        }
    }
    .theme--light.v-card{
        background-color: white;
    }
</style>