<template>
  <el-dialog
    :model-value="!authenticated"
    modal-class="dialog"
    width="400px"
    :show-close="false"
    :close-on-press-escape="false"
    :close-on-click-modal="false"
  >
    <iframe width="360" height="560" :src="iframeUrl" frameborder="0" />
  </el-dialog>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { ElDialog } from 'element-plus';
import { getBaseUrlAuth } from '@/utils';
import { getCookie } from 'typescript-cookie';

export default defineComponent({
  name: 'AuthPanel',
  components: {
    ElDialog
  },
  computed: {
    iframeUrl() {
      return `${getBaseUrlAuth()}/auth/login?inviter_id=${this.inviterId}`;
    },
    inviterId() {
      const result = this.$route.query.inviter_id?.toString() || getCookie('INVITER_ID');
      return result;
    },
    authenticated() {
      return !!this.$store.state.token.access;
    }
  },
  mounted() {
    window.addEventListener('message', async (event: MessageEvent) => {
      if (event.origin !== getBaseUrlAuth()) {
        return;
      }
      if (event.data.name === 'login') {
        const data = event.data.data;
        const token = {
          access: data.access_token,
          refresh: data.refresh_token,
          expiration: data.expires_in
        };
        await this.$store.dispatch('setToken', token);
        await this.$store.dispatch('getUser');
        window.location.reload();
      }
    });
  }
});
</script>

<style lang="scss" scoped>
.dialog {
  width: 400px;
  height: 600px;
}
</style>
