<template>
    <div>
        <header>
            <Navbar />
        </header>
        <main>
            <div class="container">
                <Message />
                <RouterView />
            </div>
        </main>
        <Footer />
    </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
import Message from "./components/Message.vue";
import { NOT_FOUND, UNAUTHORIZED, INTERNAL_SERVER_ERROR } from "./util";

export default {
    computed: {
        errorCode() {
            return this.$store.state.error.code;
        }
    },
    watch: {
        errorCode: {
            async handler(val) {
                if (val === INTERNAL_SERVER_ERROR) {
                    this.$router.push("/500");
                } else if (val === UNAUTHORIZED) {
                    // トークンをリフレッシュ
                    await axios.get("/api/refresh-token");
                    // ストアのuserをクリア
                    this.$store.commit("auth/setUser", null);
                    // ログイン画面へ
                    this.$router.push("/login");
                } else if (val === NOT_FOUND) {
                    this.$router.push("/not-found");
                }
            },
            immediate: true
        },
        $route() {
            this.$store.commit("error/setCode", null);
        }
    },
    components: {
        Message,
        Navbar,
        Footer
    }
};
</script>
