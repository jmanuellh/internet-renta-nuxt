<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            icon
            @click.stop="rightDrawer = !rightDrawer"
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-cog</v-icon>
          </v-btn>
        </template>
        <span>Ajustes</span>
      </v-tooltip>
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item class="flex justify-center">
          <v-switch
            v-model="$vuetify.theme.dark"
            label="modo nocturno"
            @change="cambioDarkMode()"
          ></v-switch>
        </v-list-item>
        <!-- <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item> -->
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Bienvenido',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Internet rentas',
          to: '/internet-rentas'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Internet servicios'
    }
  },
  mounted() {
    this.cargarDarkMode()
  },
  methods: {
    cargarDarkMode() {
      if(localStorage.getItem("internet\_renta\_nuxt.dark\_mode") == "true") {
        this.$vuetify.theme.dark = true
      } else {
        this.$vuetify.theme.dark = false
      }
    },
    cambioDarkMode() {
      localStorage.setItem("internet\_renta\_nuxt.dark\_mode", this.$vuetify.theme.dark);
    }
  },
}
</script>
