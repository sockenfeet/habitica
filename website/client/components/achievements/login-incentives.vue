<template lang="pug">
b-modal#login-incentives(:title="data.message", size='md', :hide-footer="true")
  .modal-body
    .row.reward-row
      .col-12
        avatar.avatar(:member='user', :avatarOnly='true', :withBackground='true')
      .text-center.col-12
        .reward-wrap
          div(v-if="nextReward.rewardKey.length === 1", :class="nextReward.rewardKey[0]")
          .reward(v-for="reward in nextReward.rewardKey", v-if="nextReward.rewardKey.length > 1", :class='reward')
        div(v-if="data.nextRewardAt")
          h4 {{ $t('countLeft', {count: data.nextRewardAt - user.loginIncentives}) }}
    .row
      .col-8.offset-2.text-center
        p {{ $t('incentivesDescription') }}
      .col-12.text-center(v-if="data.nextRewardAt")
        h3 {{ $t('nextRewardUnlocksIn', {numberOfCheckinsLeft: data.nextRewardAt - user.loginIncentives}) }}
  .modal-footer
    .col-12.text-center
      button.btn.btn-primary(@click='close()') {{ $t('awesome') }}
</template>

<style scoped>
  .avatar {
    width: 140px;
  }

  .reward-wrap {
    width: max-content;
  }

  .avatar, .reward-wrap {
    margin: 0 auto;
  }

  .reward-row {
    margin-top: 2em;
    margin-bottom: 2em;
  }

  .reward {
    float: left;
  }
</style>

<script>
import bModal from 'bootstrap-vue/lib/components/modal';
import { mapState } from 'client/libs/store';
import Avatar from '../avatar';
import {loginIncentives} from '../../../common/script/content/index';

export default {
  components: {
    Avatar,
    bModal,
  },
  props: ['data'],
  data () {
    return {
      loginIncentives,
    };
  },
  computed: {
    ...mapState({
      user: 'user.data',
    }),
    nextReward () {
      let nextRewardKey = this.loginIncentives[this.user.loginIncentives].nextRewardAt;
      let nextReward = this.loginIncentives[nextRewardKey];
      return nextReward;
    },
  },
  methods: {
    close () {
      this.$root.$emit('hide::modal', 'login-incentives');
    },
  },
};
</script>
