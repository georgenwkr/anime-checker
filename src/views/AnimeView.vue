<template>
  <article class="anime-details" v-show="anime">
    <p @click="previousPage" class="back-home">Back Home</p>
    <div class="details">
      <div class="img-container">
        <img :src="anime.images.jpg.image_url" alt="" />
      </div>
      <div class="information">
        <h2>
          {{ anime.title }} <span>({{ anime.aired.prop.from.year }})</span>
        </h2>
        <p>
          Rating: <span>{{ anime.rating }}</span>
        </p>
        <p>
          Score: <span>{{ anime.score }}/10</span>
        </p>
      </div>
    </div>
    <div class="synopsis">
      <h3>Synopsis:</h3>
      <p>
        {{ anime.synopsis }}
      </p>
    </div>
  </article>
</template>

<script>
import { ref } from "@vue/reactivity";
import { useRoute, useRouter } from "vue-router";
import { onBeforeMount } from "@vue/runtime-core";
export default {
  setup() {
    const route = useRoute();
    const router = useRouter();
    const anime = ref({});

    const previousPage = () => {
      router.go(-1);
    };

    onBeforeMount(async () => {
      const res = await fetch(
        `https://api.jikan.moe/v4/anime/${route.params.id}`
      );
      const data = await res.json();
      anime.value = data.data;
    });

    return { anime, previousPage };
  },
};
</script>

<style lang="scss" scoped>
.anime-details {
  width: min(90%, 320px);
  margin: 0 auto;

  @media (min-width: 900px) {
    width: min(70%, 800px);
  }

  .back-home {
    color: var(--bg-btn);
    margin: 20px 0 15px;
    cursor: pointer;
    font-size: 22px;
  }

  .details {
    margin: 12px auto;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;

    @media (min-width: 900px) {
      flex-direction: row;
      justify-content: space-between;
      align-items: flex-start;
    }

    .img-container {
      border: 1px solid transparent;
      border-radius: 8px;
      width: 300px;
      height: 420px;
      overflow: hidden;

      img {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }

    .information {
      @media (min-width: 900px) {
        width: 450px;
        line-height: 1.4;
      }
      h2 {
        font-size: 22px;
        font-weight: 600;
        color: var(--cl-text);
        margin: 5px 0;

        span {
          color: var(--cl-year);
        }
      }

      p {
        font-size: 20px;
        font-weight: 500;
        color: var(--cl-text);

        span {
          font-weight: 400;
        }
      }
    }
  }

  .synopsis {
    margin-bottom: 40px;
    line-height: 1.4;

    h3 {
      color: var(--cl-text);
      font-size: 22px;
      font-weight: 500;
      margin: 15px auto;
    }

    p {
      color: var(--cl-text);
      font-weight: 400;
      font-size: 20px;
    }
  }
}
</style>