<template>
  <div class="heading">
    <div class="heading__wrapper">
      <div class="heading__content">
        <h1 class="heading__title">MCK</h1>
        <p class="heading__desc">The goal of this project is to write a version of Minecraft in Kotlin using the latest
          libraries and
          technologies. With the end goal of being the highest performance Minecraft client for PC
          <span class="heading__note">The statistics on this website are automatically updated from source.</span>
        </p>
        <p class="heading__date">Updated at {{ changedAt }}</p>
        <div class="heading__buttons">
          <a href="https://github.com/MinecraftKotlin" target="_blank" rel="noopener" class="heading__github">
            <GithubIcon class="heading__github__icon"/>
            View on Github
          </a>
        </div>
      </div>
      <LogoGraph :percent="percentRewritten"/>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { DateTime } from "luxon"

export default Vue.extend({
  name: 'Heading',
  props: { updatedAt: Number, percentRewritten: Number },
  data() {
    const date = DateTime.fromMillis(this.updatedAt)
    return {
      changedAt: date.toFormat('fff')
    }
  }
})
</script>

<style scoped lang="scss">
.heading {
  max-width: 900px;
  padding: 2rem;
  margin: 5rem auto;

  &__content {
    animation: heading 1s ease;
    max-width: 500px;
    flex: auto;
  }

  &__wrapper {
    display: flex;
    align-items: center;
    flex-wrap: wrap-reverse;
    justify-content: space-between;
  }

  &__title {
    font-size: 3rem;
    margin-bottom: 0.5rem;
  }

  &__desc {
    font-size: 1.2rem;
    line-height: 1.5;
    color: #e7e7e7;
  }

  &__note {
    display: block;
    margin-top: 0.5em;
    font-size: 1rem;
    color: #555;
  }

  &__date {
    margin: 0.5rem 0;
    color: #9653c4;
  }

  &__buttons {
    display: inline-block;
  }

  &__github {
    padding: 0.5rem;
    border-radius: 0.5rem;
    border: 2px solid #FFFFFF;
    color: #FFFFFF;
    display: inline-block;
    text-decoration: none;
    margin-top: 1rem;

    &__icon {
      width: 32px;
      height: 32px;
      vertical-align: middle;
      margin-right: 0.5rem;
    }
  }

}

@keyframes heading {
  0% {
    transform: translateX(-25%);
    opacity: 0;
  }
  100% {
    transform: translateX(0);
    opacity: 1;
  }
}

</style>
