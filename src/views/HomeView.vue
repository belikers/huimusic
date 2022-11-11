<template>
  <div class="home">
    <div class="w1200">
      <!-- 首页轮播图部分 -->
      <my-banner></my-banner>
      <!-- 排行榜 -->
      <div class="top_list">
        <div class="h_title">
          <h3>排行榜</h3>
        </div>
        <el-row class="wrapper" :gutter="20">
          <el-col
            v-for="item in top_list"
            :key="item.id"
            :span="6"
            class="toplist_item"
          >
            <div class="coverImg">
              <el-image :src="item.coverImgUrl"></el-image>
            </div>
            <div class="toplist_wrapper">
              <h4 class="toplist_hd">{{ item.name }}</h4>
              <div class="toplist_songlist">
                <div
                  v-for="(songItem, index) in songList[item.id]"
                  :key="songItem.id"
                  class="songitem"
                >
                  <div class="songnum">{{ index + 1 }}</div>
                  <div class="songinfo">
                    <router-link
                      :to="{ path: '/song', query: { id: songItem.id } }"
                      class="song_title"
                      >{{ songItem.name }}</router-link
                    >
                    <div class="song_author">
                      <router-link
                        v-for="(author, k) in songItem.ar"
                        :key="k"
                        :to="{
                          path: '/artist/Singer',
                          query: { id: author.id },
                        }"
                        class="song_name"
                        >{{
                          k !== 0 ? " / " + author.name : author.name
                        }}</router-link
                      >
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </el-col>
        </el-row>
      </div>
      <!-- 热门歌曲：playlist模块 -->
      <div class="recom_list">
        <div class="h_title">
          <h3>热门歌曲</h3>
          <span
            v-for="(item, index) in playlist_tags"
            :key="item.id"
            :class="index === playlist_index ? 'active' : ''"
            @click="choosePlayListType(index)"
            >{{ item.name }}</span
          >
        </div>
        <div class="wrapper">
          <play-list :play-list="playlist_list"></play-list>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts"></script>
<style lang="less" scoped>
.h_title {
  padding: 20px 0 0;
  text-align: center;

  h3 {
    padding: 20px 0;
    font-size: 28px;
    font-weight: 700;
  }

  span {
    display: inline-block;
    font-size: 16px;
    margin: 0 24px;
    color: #333;
    cursor: pointer;

    &.active {
      position: relative;
      font-weight: 600;
      color: #000;
      &:after {
        position: absolute;
        content: "";
        left: 0;
        bottom: 1px;
        width: 100%;
        height: 6px;
        background: #ff641e;
        z-index: -1;
      }
    }
  }
}

.wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.toplist_item {
  position: relative;
  margin-bottom: 30px;

  .coverImg {
    position: absolute;
    top: 0;
    right: 10px;
    left: 10px;
    z-index: 1;
    height: 100%;

    &::before {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 2;
      content: "";
      width: 100%;
      height: 100%;
    }

    /deep/ img {
      filter: blur(60px);
      transform: scale(1.5);
    }
  }

  .toplist_wrapper {
    position: relative;
    z-index: 2;
  }

  &:hover {
    /deep/ img {
      transform: scale(2);
    }
  }
}

.toplist_wrapper {
  padding: 30px 40px;
  height: 505px;
  background-color: rgba(168, 163, 163, 0.1);

  .toplist_hd {
    position: relative;
    font-size: 24px;
    text-align: center;
    color: #fff;

    &::after {
      display: inline-block;
      content: "";
      position: absolute;
      bottom: -30px;
      left: 0;
      right: 0;
      width: 30px;
      height: 2px;
      margin: 0 auto;
      background-color: #fff;
    }
  }

  .toplist_songlist {
    padding-top: 80px;
  }

  .songitem {
    display: flex;
    padding-bottom: 20px;
    color: #fff;

    .songnum {
      padding-right: 10px;
      font-size: 18px;
    }

    .song_title {
      line-height: 24px;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      word-break: break-all;
      padding-bottom: 5px;
      font-size: 14px;
      color: #fff;
    }

    .song_author {
      display: block;
      font-size: 0;
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      word-break: break-all;

      a {
        display: inline-block;
        line-height: 20px;
        font-size: 14px;
        color: #fff;
      }
    }

    &:last-child {
      padding-bottom: 0;
    }
  }
}

.artists_item {
  width: 120px;

  .el-image {
    transition: all 0.4s linear;
  }

  &:hover {
    .el-image {
      transform: rotateY(180deg);
    }
  }

  .faceImg {
    width: 120px;
    height: 120px;
    border-radius: 100%;
    overflow: hidden;
  }

  .info {
    text-align: center;

    .name {
      line-height: 28px;
      font-size: 14px;
    }

    .albumSize {
      color: #999;
    }
  }
}

.artists_list {
  padding-bottom: 40px;
  margin-bottom: 50px;
}
</style>
