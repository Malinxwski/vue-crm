<template>
    <div >
        <Loader v-if="loading" />
        <div class="app-main-layout grey lighten-2 " v-else>

            <Navbar  @toggle="isOpen = !isOpen" />

            <Sidebar class=" teal darken-2 " v-model="isOpen" />

            <main class="app-content  " :class="{full: !isOpen}">
                <div class="app-page  ">
                    <router-view />
                </div>
            </main>

            <div class="fixed-action-btn"
                 v-tooltip="'Cоздать запись'"
            >
                <router-link class="btn-floating btn-large blue"  to="/record">
                    <i class="large material-icons"
                    >add</i>
                </router-link>
            </div>
        </div>
    </div>
</template>
<style scoped lang="css">


</style>

<script>
    import Navbar from '../components/app/Navbar'
    import Sidebar from '../components/app/Sidebar'
    import messages from "../utils/messages";

    export default {
        name: 'main-layout',
        data: () => ({
            isOpen: true,
            loading: true
        }),
        async mounted() {
            if (!Object.keys(this.$store.getters.info).length) {
                await this.$store.dispatch('fetchInfo')
            }

            this.loading = false
        },
        components: {
            Navbar, Sidebar
        },
        computed: {
            error() {
                return this.$store.getters.error
            }
        },

        watch: {
            error(fbError) {
                this.$error(messages[fbError.code] || 'Что-то пошло не так')
            }
        }
    }
</script>
