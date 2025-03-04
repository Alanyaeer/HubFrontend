<template>
  <div class="navigator">
    <div class="top">
      <div v-for="(link, linkIndex) in links" :key="linkIndex" class="link">
        <el-tooltip effect="dark" :content="link.displayName" placement="right">
          <el-button
            :class="{
            button: true,
            active: link.routes.includes($route.name as string)
          }"
            @click="$router.push(link.route)"
          >
            <font-awesome-icon :icon="link.icon" />
          </el-button>
        </el-tooltip>
      </div>
    </div>
    <div class="middle" />
    <div class="bottom">
      <div class="link">
        <el-tooltip effect="dark" :content="$t('common.nav.console')" placement="right">
          <el-button class="button" @click="onConsole">
            <font-awesome-icon icon="fa-solid fa-compass" />
          </el-button>
        </el-tooltip>
      </div>
      <div class="link">
        <help-entry />
      </div>
      <div v-if="authenticated" class="link">
        <el-tooltip effect="dark" :content="$t('common.nav.logOut')" placement="right">
          <el-button class="button" @click="onLogout">
            <font-awesome-icon icon="fa-solid fa-arrow-right-from-bracket" />
          </el-button>
        </el-tooltip>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { ElButton, ElTooltip } from 'element-plus';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import {
  ROUTE_CHAT_CONVERSATION,
  ROUTE_CHAT_CONVERSATION_NEW,
  ROUTE_CONSOLE_ROOT,
  ROUTE_MIDJOURNEY_HISTORY,
  ROUTE_MIDJOURNEY_INDEX
} from '@/router/constants';
import HelpEntry from './HelpEntry2.vue';

export default defineComponent({
  name: 'Navigator',
  components: {
    ElButton,
    HelpEntry,
    ElTooltip,
    FontAwesomeIcon
  },
  data() {
    return {
      links: [
        {
          route: {
            name: ROUTE_CHAT_CONVERSATION_NEW
          },
          displayName: this.$t('common.nav.chat'),
          icon: 'fa-regular fa-comment',
          image: 'https://cdn.zhishuyun.com/9ad12c99b2.png/thumb_100x100',
          routes: [ROUTE_CHAT_CONVERSATION, ROUTE_CHAT_CONVERSATION_NEW]
        },
        {
          route: {
            name: ROUTE_MIDJOURNEY_INDEX
          },
          displayName: this.$t('common.nav.midjourney'),
          icon: 'fa-solid fa-palette',
          image: 'https://cdn.zhishuyun.com/83ee211091.png/thumb_100x100',
          routes: [ROUTE_MIDJOURNEY_INDEX, ROUTE_MIDJOURNEY_HISTORY]
        }
      ]
    };
  },
  computed: {
    authenticated() {
      return !!this.$store.state.token.access;
    }
  },
  methods: {
    async onLogout() {
      await this.$store.dispatch('resetAll');
      await this.$store.dispatch('chat/resetAll');
      await this.$store.dispatch('midjourney/resetAll');
    },
    onConsole() {
      this.$router.push({ name: ROUTE_CONSOLE_ROOT });
    }
  }
});
</script>

<style lang="scss" scoped>
.navigator {
  display: flex;
  flex-direction: column;
  align-items: center;

  .top,
  .bottom {
    display: flex;
    flex-direction: column;
    padding-top: 10px;
    .link {
      width: 40px;
      height: 40px;
      margin-bottom: 10px;
      .button {
        width: 40px;
        height: 40px;
        border-radius: 10px;
        border: none;
        background-color: var(--el-bg-color-page);
        &.active,
        &:hover,
        &:focus {
          background-color: var(--el-button-hover-bg-color);
          color: var(--el-button-active-text-color);
        }
      }
    }
  }
  .bottom {
    display: block;
    position: absolute;
    bottom: 0;
  }
}
</style>
