<template>
    <v-dialog v-model="myPageDialog" persisten max-width="400px" >
           <template v-slot:activator="{ on }">
            <v-btn color="cyan lighten-3" style="font-size: 0.7em;" dark v-on="on">
                <v-icon style="font-size: 1.3em;">
                    mdi-account
                </v-icon>
                    Mypage
            </v-btn>  
        </template>
    <v-card dark>
            <v-card-title class="headline">
        <span>
            MyPage
        </span>
            </v-card-title>
    </v-card>
        <v-card-text class="grey lighten-5" dense dark>
            <v-container>
                <v-row>
                    <v-text-field  class="pl-3 pr-3" v-model="userId"  label = "userId"
                         disabled type="text" prepend-icon="mdi-account" flat solo>    
                    </v-text-field>
                    <v-text-field  class="pl-3 pr-3"  :rules="passwordRules" required  v-model="password"
                        label="비밀번호변경" type="password" prepend-icon="mdi-lock" flat solo>
                    </v-text-field>
                    <v-text-field  class="pl-3 pr-3"  :rules="passwordCheck" required  v-model="passwordChecking"
                        label="비밀번호확인" type="password" prepend-icon="mdi-lock" flat solo>
                    </v-text-field>
                    <v-text-field class="pl-3 pr-3" v-model="email"  label = "email"
                        disabled type="text" prepend-icon="mdi-email-multiple" flat solo>
                    </v-text-field>
                    <v-text-field class="pl-3 pr-3" v-model="nickname"  label = "nickname"
                        disabled type="text" prepend-icon="mdi-account" flat solo>
                    </v-text-field>
                </v-row>
            </v-container>
        </v-card-text>
            <v-card-actions class="grey lighten-5">
                    <v-spacer></v-spacer> 
                <v-dialog v-model="memberRemove" persisten max-width="500px">
                    <template v-slot:activator="{ on }">
                            <v-btn text class="red--text" v-on="on">
                                탈퇴
                            </v-btn>  
                        </template>
                        <v-card dark >
                            <v-container>
                                    <v-btn text class="red--text" @click="remove()">
                                        탈퇴
                                    </v-btn>
                                    <v-btn text @click="memberRemove = false">
                                        취소    
                                    </v-btn>
                            </v-container>
                        </v-card>
                </v-dialog>
                        <v-btn class="grey--text" text @click="myPageDialog = false">
                            취소
                        </v-btn>
                        <v-btn text @click="modify()">
                            변경
                        </v-btn> 

            </v-card-actions>   
    </v-dialog>
</template>

<script>
import axios from 'axios'
import { mapState } from 'vuex'
export default {
    name: 'MemberMyPage',
    props: {
        members: {
            type: Array
        }
    },
    data () {
        return {
            myPageDialog: false,
            memberRemove:false,
            userId:'',
            password:'',
            nickname: '',
            email: '',
            memberNo:'',
            regDate:'',
            passwordChecking: '',
            passwordCheck: [
            v => this.password ===v || '비밀번호가 일치하지않습니다'
            ]
        }
    },
     created () {
        this.userId = this.$store.state.userInfo.userId
        this.nickname = this.$store.state.userInfo.nickname
        this.email = this.$store.state.userInfo.email
        
    },
    computed :{
        ...mapState(['passwordRules'])
    },
    methods: {
        modify () {
            const { password } = this
            for(var i = 0; i < this.members.length; i++){
                if(this.members[i].userId === this.userId){
                    if(password.length >= 8 && password.length <= 15) {   
                        axios.put(`http://localhost:7777/member/modify`,
                        { userId: this.userId, password, nickname: this.nickname, email: this.email, 
                        regDate: this.members[i].regDate, memberNo: this.members[i].memberNo })
                            .then(() => {
                                alert('비밀번호 변경 성공! 로그아웃 됩니다')
                                this.$cookies.remove("user")
                                this.$store.state.isLogin = false
                                this.$store.state.userInfo = null
                                this.$router.go()
                                
                                
                                })
                            .catch(() => {
                                alert('게시물 수정 실패!')
                            })
                    }else{
                        alert('비밀번호는 8~15자 입니다.')
                    }
                }
            }
        },
        
        remove () {
            for(var i = 0; i < this.members.length; i++){
                if(this.members[i].userId === this.userId){
                        axios.delete(`http://localhost:7777/member/remove`,{ data:{ memberNo: this.members[i].memberNo } })
                        .then(() => {
                            alert('회원 탈퇴 되었습니다.')
                            this.$cookies.remove("user")
                            this.$store.state.isLogin = false
                            this.$store.state.userInfo = null
                            this.$router.go()
                            
                            
                        })
                        .catch(() => {
                            alert('문제 발생!')
                        })
                }
            }
        }
    }
}
</script>