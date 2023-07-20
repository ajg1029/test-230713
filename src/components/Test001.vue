<template>
  <div>
    <div>Test001</div>

    <br />

    <div v-if="!isLoggedIn">
      <div style="font-weight: 600;">회원가입</div>
      <div>이메일 <input v-model="createUserEmail"></div>
      <div>비밀번호 <input v-model="createUserPassword" type="password"></div>
      <div><button @click="createFirebaseUser(createUserEmail, createUserPassword)">회원가입</button></div>
      <br />
    </div>

    <div v-if="!isLoggedIn">
      <div style="font-weight: 600;">로그인</div>
      <div>이메일 <input v-model="loginUserEmail"></div>
      <div>비밀번호 <input v-model="loginUserPassword" type="password"></div>
      <div><button @click="loginFirebase(loginUserEmail, loginUserPassword)">로그인</button></div>
      <br />
    </div>

    <div v-if="isLoggedIn">
      <div>{{ this.currentUser?.email }}</div>
      <div><button @click="logoutFirebase">로그아웃</button></div>
      <br />
    </div>
    
    <div>
      <div>isLoggedIn : {{ isLoggedIn }}</div>
      <br />
    </div>

    <!-- <div>
      <div><button @click="testButton">testButton</button></div>
      <br />
    </div> -->

    <div>
      <div>user01@naver.com / cldstn302</div>
      <div>user02@naver.com / 123qwe</div>
      <div>user03@naver.com / 123qwe</div>
      <div>user04@naver.com / 123qwe</div>
      <br />
    </div>

    <div>
      <div><button @click="openFirestoreCollection('test001')">openFirestoreCollection</button></div>
      <br />
    </div>

    <div>
      <div><button @click="closeFirestoreCollection">closeFirestoreCollection</button></div>
      <br />
    </div>

    <div>
      <div>inputDocId <input v-model="inputDocId"></div>
      <div>inputDocDataName <input v-model="inputDocDataName"></div>
      <div>inputDocDataNum <input v-model="inputDocDataNum"></div>
      <div><button @click="addFirestoreDoc">addFirestoreDoc</button></div>
      <br />
    </div>
    
  </div>
</template>

<script>
import { initializeApp } from "firebase/app";
import { getFirestore, collection, query, where, onSnapshot, doc, setDoc } from "firebase/firestore";
import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "firebase/auth";

const firebaseConfig = {
  apiKey: "AIzaSyBuNui09yAQg89IZ903mgYSBesGMHRcq_c",
  authDomain: "test-230713-c3f76.firebaseapp.com",
  projectId: "test-230713-c3f76",
  storageBucket: "test-230713-c3f76.appspot.com",
  messagingSenderId: "862199087483",
  appId: "1:862199087483:web:e2fbc80aa30b6431b4bdde"
};
const firebaseApp = initializeApp(firebaseConfig);
const firestoreDB = getFirestore(firebaseApp)

// Firebase 호스팅으로 사이트를 호스팅하려면 Firebase CLI(명령줄 도구)가 필요합니다.
// 다음 npm 명령어를 실행하여 CLI를 설치하거나 최신 CLI 버전으로 업데이트하세요.
// npm install -g firebase-tools

// 지금 또는 나중에 배포할 수 있습니다. 지금 배포하려면 터미널 창을 연 다음 웹 앱의 루트 디렉터리로 이동하거나 루트 디렉터리를 만드세요.
// Google에 로그인
// firebase login

// 프로젝트 시작
// 앱의 루트 디렉터리에서 다음 명령어를 실행하세요.
// firebase init

// 준비되면 웹 앱 배포
// 정적 파일(예: HTML, CSS, JS)을 앱의 배포 디렉터리에 넣으세요. 기본값은 '공개'입니다. 그런 다음 앱의 루트 디렉터리에서 이 명령어를 실행하세요.
// firebase deploy

// 배포 후 test-230713-c3f76.web.app에서 앱을 확인하세요.

export default {
  name: 'Test001',
  data() {
    return {
      unsubscribe: null,

      createUserEmail: "",
      createUserPassword: "",

      isLoggedIn: false,

      loginUserEmail: "",
      loginUserPassword: "",
      
      currentUser: null,

      inputDocId: "",
      inputDocDataName: "",
      inputDocDataNum: "",

    }
  },
  mounted() {
    // this.openFirestoreCollection("test001")
    // console.log(getAuth())
    this.checkIsLoggedIn()
  },
  methods: {
    openFirestoreCollection(collectionName) {
      const q = query(collection(firestoreDB, collectionName), where("num", "<", 10))
      this.unsubscribe = onSnapshot(q, (querySnapshot) => {
        const arr = []
        querySnapshot.forEach((doc) => {
          arr.push(doc.data())
        })
        console.log(arr)
      })
    },
    closeFirestoreCollection() {
      this.unsubscribe()
    },
    async addFirestoreDoc() {
      await setDoc(doc(firestoreDB, "test001", this.inputDocId), {
        name: this.inputDocDataName,
        num: parseInt(this.inputDocDataNum),
      })
      this.inputDocId = ""
      this.inputDocDataName = ""
      this.inputDocDataNum = ""
    },
    createFirebaseUser(email, password) {
      if (this.isLoggedIn) {
        return
      }
      const auth = getAuth()
      createUserWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
          this.createUserEmail = ""
          this.createUserPassword = ""
          this.currentUser = userCredential.user
          this.isLoggedIn = true
        })
        .catch((error) => {
          this.createUserEmail = ""
          this.createUserPassword = ""
          this.currentUser = null
          this.isLoggedIn = false
          console.log(error.code, error.message)
        })
    },
    loginFirebase(email, password) {
      if (this.isLoggedIn) {
        return
      }
      const auth = getAuth()
      signInWithEmailAndPassword(auth, email, password)
        .then((userCredential) => {
          this.loginUserEmail = ""
          this.loginUserPassword = ""
          this.currentUser = userCredential.user
          this.isLoggedIn = true
        })
        .catch((error) => {
          this.loginUserEmail = ""
          this.loginUserPassword = ""
          this.currentUser = null
          this.isLoggedIn = false
          console.log(error.code, error.message)
        })
    },
    logoutFirebase() {
      if (!this.isLoggedIn) {
        return
      }
      const auth = getAuth()
      signOut(auth).then(() => {
        this.isLoggedIn = false
      }).catch((error) => {
        console.log(error)
      })
    },
    // testButton() {
    //   const auth = getAuth()
    //   console.log(auth.currentUser)
    // }
    checkIsLoggedIn() {
      const auth = getAuth()
      if (auth.currentUser) {
        this.isLoggedIn = true
      } else {
        this.isLoggedIn = false
      }
    }
  }
}

</script>

<style scoped>

</style>
