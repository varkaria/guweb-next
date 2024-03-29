<template>
  <nav class="z-20 w-full transition-all lg:fixed bg-hsl-h2 lg:bg-transparent">
    <div class="px-2 mx-auto max-w-7xl sm:px-6 lg:px-8">
      <div class="relative flex items-center justify-between h-16">
        <div class="absolute inset-y-0 left-0 flex items-center sm:hidden">
          <button
            type="button"
            class="inline-flex items-center justify-center p-2 text-gray-400 rounded-md hover:text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white"
            aria-controls="mobile-menu"
            aria-expanded="false"
            @click="toggleMenuResponsive"
          >
            <svg
              class="block w-6 h-6"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
              aria-hidden="true"
              :class="{ isopen: isMobileOpen }"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 6h16M4 12h16M4 18h16"
              />
            </svg>
          </button>
        </div>
        <div
          class="flex items-center justify-center flex-1 sm:items-stretch sm:justify-start"
        >
          <div class="flex items-center flex-shrink-0">
            <nuxt-link to="/">
              <h1 class="text-xl font-bold text-white">
                guweb
              </h1>
            </nuxt-link>
          </div>
          <div class="hidden sm:block sm:ml-6">
            <div class="flex space-x-2">
              <nuxt-link
                v-for="(item, index) in menuItems.main"
                :key="index"
                :to="item.url"
              >
                <a
                  href="#"
                  class="px-3 py-2 text-sm font-medium text-white rounded-md hover:bg-black transition-all hover:bg-opacity-20 hover:backdrop-blur-lg"
                  aria-current="page"
                >{{ item.name }}</a>
              </nuxt-link>
            </div>
          </div>
        </div>
        <div v-click-outside="closeMenu" class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0">
          <div class="ml-3 relative">
            <div>
              <button
                id="user-menu-button"
                type="button"
                class="flex text-sm rounded-full transition-all items-center gap-2 text-white"
                aria-expanded="false"
                aria-haspopup="true"
                @click="toggleMenu"
              >
                <span class="sr-only">Open user menu</span>
                <template v-if="$auth.loggedIn">
                  <img class="h-8 w-8 rounded-full" :src="`https://a.${baseURL}/${$auth.user.id}`">
                  <h1 class="font-semibold">
                    Hi {{ $auth.user.name }}
                  </h1>
                </template>
                <template v-else>
                  <img class="h-8 w-8 rounded-full" :src="`https://a.${baseURL}/0`">
                </template>
              </button>
            </div>

            <transition
              enter-active-class="transition-all duration-100 transform ease-out"
              enter-class="opacity-0 -translate-y-2"
              enter-to-class="opacity-100"
              leave-active-class="transition-all duration-100 transform ease-in"
              leave-class="opacity-100"
              leave-to-class="opacity-0 -translate-y-2"
            >
              <div
                v-show="isMenuOpen"
                class="origin-top-right absolute right-0 mt-2 w-[10rem] rounded-md shadow-lg py-1 bg-hsl-b3 ring-1 ring-black ring-opacity-5 focus:outline-none"
                role="menu"
                aria-orientation="vertical"
                aria-labelledby="user-menu-button"
                tabindex="-1"
              >
                <template v-if="!$auth.loggedIn">
                  <nuxt-link
                    v-for="(m, index) in menuItems.guest"
                    :key="index"
                    :to="m.url"
                    class="block px-4 py-2 text-sm text-white hover:border-l-2 transition-all font-semibold border-hsl-h2"
                    role="menuitem"
                    tabindex="-1"
                    @click.native="closeMenu"
                  >
                    {{ m.name }}
                  </nuxt-link>
                </template>
                <template v-else>
                  <nuxt-link
                    v-for="(m, index) in menuItems.user"
                    :key="index"
                    :to="m.url"
                    class="block px-4 py-2 text-sm text-white hover:border-l-2 transition-all font-semibold border-hsl-h2"
                    role="menuitem"
                    tabindex="-1"
                    @click.native="closeMenu"
                  >
                    {{ m.name }}
                  </nuxt-link>
                </template>
              </div>
            </transition>
          </div>
        </div>
      </div>

      <div v-show="isMobileOpen" id="mobile-menu" class="sm:hidden">
        <div class="px-2 pt-2 pb-3 space-y-1">
          <a
            v-for="(item, index) in menuItems.main"
            :key="index"
            href="#"
            class="block px-3 py-2 text-base font-medium text-white rounded-md"
            aria-current="page"
          >{{ item.name }}</a>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import vClickOutside from 'v-click-outside'
export default {
  name: 'NavbarDefault',
  directives: {
    clickOutside: vClickOutside.directive
  },
  data () {
    return {
      isMenuOpen: false,
      isMobileOpen: false,
      menuItems: {
        main: [
          { name: 'Home', url: '/' },
          { name: 'Leaderboard', url: '/leaderboard' }
        ],
        user: [
          { name: 'Profile', url: '/dashboard' },
          { name: 'Settings', url: '/dashboard' },
          { name: 'Sign out', url: '/auth/logout' }
        ],
        guest: [
          { name: 'Sign in', url: '/auth/login' },
          { name: 'Sign up', url: '/auth/register' }
        ]
      },
      baseURL: process.env.baseUrl
    }
  },
  methods: {
    closeMenu () {
      this.isMenuOpen = false
    },
    toggleMenu () {
      this.isMenuOpen = !this.isMenuOpen
    },
    toggleMenuResponsive () {
      this.isMobileOpen = !this.isMobileOpen
    }
  }
}
</script>

<style>
.isopen path {
  transition: 0.2s;
  d: path("M6 18L18 6M6 6l12 12");
}
</style>
