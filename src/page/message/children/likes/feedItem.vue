<template>
  <section>
    <div :class="`${prefixCls}-item-top`" class="m-box m-aln-center m-justify-bet">
      <avatar :user="user" />
      <section class="userInfo m-flex-grow1 m-flex-shrink1 m-flex-base0">
        <span v-if="!user.id" :class="`${prefixCls}-item-top-link`">未知用户</span>
        <router-link :class="`${prefixCls}-item-top-link`" :to="`/users/${user.id}`">{{ user.name }}</router-link>
        <span> 赞了你的动态</span>
        <p>{{ like.created_at | time2tips }}</p>
      </section>
      <svg class="m-style-svg m-svg-def m-flex-grow0 m-shrink0">
        <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#feed-like"></use>
      </svg>
    </div>
    <div :class="`${prefixCls}-item-bottom`">
      <section v-if="like.likeable !== null" @click="goToFeedDetail()">
        <div :class="`${prefixCls}-item-bottom-noImg`" class="content" v-if="!getImage && !getVideo">
          {{ like.likeable.feed_content }}
        </div>
        <div :class="`${prefixCls}-item-bottom-img`" v-else>
          <div class="img">
            <img v-if="getImage" :src="getImage" :alt="user.name" />
            <img v-if="getVideo" :src="getVideo.cover" :alt="user.name" />
          </div>
          <div class="content">
            {{ like.likeable.feed_content }}
          </div>
        </div>
      </section>
      <section v-if="like.likeable === null">
        <div :class="`${prefixCls}-item-bottom-noImg`" class="content">
          动态已被删除
        </div>
      </section>
    </div>
  </section>
</template>
<script>
const prefixCls = "msgList";
export default {
  name: "feedsItem",
  props: ["like"],
  data: () => ({
    prefixCls
  }),
  methods: {
    /**
     * 进入动态详情
     * @Author   Wayne
     * @DateTime 2018-01-31
     * @Email    qiaobin@zhiyicx.com
     * @return   {[type]}            [description]
     */
    goToFeedDetail() {
      const {
        likeable: { id = 0 }
      } = this.like;
      this.$router.push(`/feeds/${id}`);
    }
  },
  computed: {
    /**
     * 获取图片,并计算地址
     * @Author   Wayne
     * @DateTime 2018-01-31
     * @Email    qiaobin@zhiyicx.com
     * @return   {[type]}            [description]
     */
    getImage() {
      const { like } = this;
      const { length } = like.likeable.images;
      if (length > 0) {
        const { 0: img = {} } = like.likeable.images;
        return `/api/v2/files/${img.id}`;
      }

      return false;
    },
    getVideo() {
      const { like } = this.$props;
      const video = like.likeable.video;
      if (video !== null) {
        return {
          video: `/api/v2/files/${video.video_id}`,
          cover: `/api/v2/files/${video.cover_id}`
        };
      }
      return false;
    },
    user() {
      return this.like.user || {};
    }
  },
  created() {
    // console.log(this.comment)
  }
};
</script>
