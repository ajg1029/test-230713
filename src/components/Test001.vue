<template>
  <div>
    <div>Test001</div>
  </div>
</template>

<script>
import { initializeApp } from "firebase/app";
import { getFirestore, collection, query, where, onSnapshot } from "firebase/firestore";

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
      unsubscribe: null
    }
  },
  created() {
    
  },
  mounted() {
    // this.openFirestoreCollection("test001")
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
    }
  }
}

</script>

<style scoped>

</style>