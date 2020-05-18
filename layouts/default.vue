<template>
    <v-app app dark>
        <v-navigation-drawer app v-model="drover" mobile-break-point="650">
            <v-list subheader>
                <v-subheader>Recent chat</v-subheader>

                <v-list-tile v-for="item in users" :key="item.id" @click.prevent>

                    <v-list-tile-content>
                        <v-list-tile-title
                            v-text="item.name"
                        ></v-list-tile-title>
                    </v-list-tile-content>

                    <v-list-tile-action>
                        <v-icon
                            :color="
                                item.id ? 'deep-purple accent-4' : 'grey'
                            "
                            >chat_bubble</v-icon
                        >
                    </v-list-tile-action>
                </v-list-tile>
            </v-list>
        </v-navigation-drawer>

        <v-toolbar app>
            <v-toolbar-side-icon
                @click="drover = !drover"
            ></v-toolbar-side-icon>
            <v-btn icon @click="exit">
                <v-icon>arrow_back</v-icon>
            </v-btn>
            <v-toolbar-title>Chat Room {{ user.room }}</v-toolbar-title>
        </v-toolbar>

        <v-content>
            <div style="height: 100%">
                <nuxt />
            </div>
        </v-content>
    </v-app>
</template>

<script>
import { mapState, mapMutations } from "vuex";
export default {
    computed: mapState(["user", 'users']),
    methods: {
        ...mapMutations(['clearData']),
        exit() {
            this.$socket.emit('userLeft', this.user.id, () => {
                this.$router.push('/')
                this.clearData()
            })
            
        }
    },
    data: () => ({
        drover: true,
    })
};
</script>
