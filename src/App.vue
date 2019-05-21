<template>
    <b-container id="app">
        <b-row>
            <b-col>
                <b-nav>
                    <b-nav-item :to="{name :'home'}">Home</b-nav-item>
                    <b-nav-item :to="{name :'about'}">About</b-nav-item>
                    <b-nav-item :to="{name :'linear'}">Linear</b-nav-item>
                </b-nav>
            </b-col>
        </b-row>
        <b-row>
            <b-col>
                <router-view/>
            </b-col>
        </b-row>

    </b-container>
</template>

<style lang="scss">
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
    }
</style>
