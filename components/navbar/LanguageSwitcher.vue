<template>
  <div class="relative">
    <button
      href="#"
      class="flex items-center language-switcher"
      style="outline: none"
      @click="toggleVisibility"
      @keydown.space.exact.prevent="toggleVisibility"
      @keydown.esc.exact="hideDropdown"
      @keydown.shift.tab="hideDropdown"
      @keydown.up.exact.prevent="startArrowKeys"
      @keydown.down.exact.prevent="startArrowKeys"
    >
      <img
        :src="require(`~/assets/svgs/${$i18n.locale}.svg`)"
        class="w-8 h-8"
      />
      <span class="ml-2 d-flex py-2">
        {{ $i18n.locale }}
      </span>
      <svg
        fill="currentColor"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        width="24"
        height="24"
      >
        <path
          class="heroicon-ui"
          d="M15.3 9.3a1 1 0 0 1 1.4 1.4l-4 4a1 1 0 0 1-1.4 0l-4-4a1 1 0 0 1 1.4-1.4l3.3 3.29 3.3-3.3z"
        ></path>
      </svg>
    </button>
    <transition name="dropdown-fade">
      <ul
        v-on-clickaway="hideDropdown"
        v-if="isVisible"
        ref="dropdown"
        class="list-wrap absolute normal-case z-30 font-normal xs:left-0 lg:right-0 bg-white shadow overflow-hidden rounded w-48 border mt-2 py-1 lg:z-20"
      >
        <li @click="hideDropdown" style="z-index: 9999">
          <nuxt-link
            :to="switchLocalePath(locale.code)"
            v-for="(locale, i) in showLocales"
            :key="i"
            class="d-flex"
          >
            <span class="ml-2">{{ locale.name }}</span>
          </nuxt-link>
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'LangSwitcher',
  data() {
    return {
      isVisible: false,
      focusedIndex: 0,
    }
  },
  methods: {
    toggleVisibility() {
      this.isVisible = !this.isVisible
    },
    hideDropdown() {
      this.isVisible = false
      this.focusedIndex = 0
    },
    startArrowKeys() {
      if (this.isVisible) {
        this.$refs.dropdown.children[0].children[0].focus()
      }
    },
    focusPrevious(isArrowKey) {
      this.focusedIndex = this.focusedIndex - 1
      if (isArrowKey) {
        this.focusItem()
      }
    },
    focusNext(isArrowKey) {
      this.focusedIndex = this.focusedIndex + 1
      if (isArrowKey) {
        this.focusItem()
      }
    },
    focusItem() {
      this.$refs.dropdown.children[this.focusedIndex].children[0].focus()
    },
    setLocale(locale) {
      this.$i18n.locale = locale
      this.$router.push({
        params: { lang: locale },
      })
      this.hideDropdown()
    },
  },
  computed: {
    showLocales() {
      // console.log(
      //   'showlocales',
      //   this.$i18n.locales.filter((locale) => locale.code !== this.$i18n.locale)
      // )
      return this.$i18n.locales.filter(
        (locale) => locale.code !== this.$i18n.locale
      )
    },
  },
}
</script>

<style scoped>
.dropdown-fade-enter-active,
.dropdown-fade-leave-active {
  transition: all 0.1s ease-in-out;
}
.dropdown-fade-enter,
.dropdown-fade-leave-to {
  opacity: 0;
  transform: translateY(-12px);
}
.list-wrap {
  z-index: 9999;
  width: 100px;
}
</style>
