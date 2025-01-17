<template>
  <Page
    :class="[
      'relative',
      'justify-start',
      'items-center',
      'bg-light-gray',
      'mt-[-100px]',
      'pt-[100px]',
    ]"
  >
    <div
      :class="[
        'flex',
        'flex-col',
        'flex-grow',
        'items-center',
        'w-full',
        'mx-auto',
        'px-[16px]',
        'bg-white',
        'sm:px-[24px]',
        'lg:bg-gradient-to-b',
        'lg:from-white',
        'lg:to-light-gray',
        'lg:mt-[-100px]',
      ]"
    >
      <!-- backgroung images -->
      <img
        :class="[
          'hidden',
          'absolute',
          'z-[0]',
          'w-full',
          'top-[100px]',
          'max-w-[1280px]',

          'sm:block',
          'lg:top-[-50px]',
          '2xl:top-0',
          '2xl:max-w-[1440px]',
        ]"
        src="/images/airdrop/background.png"
      />
      <img
        :class="[
          'absolute',
          'z-[0]',
          'top-[100px]',
          'w-full',
          'sm:hidden',
        ]"
        src="/images/airdrop/background_sm.png"
      />
      <img
        :class="[
          'absolute',
          'z-[0]',
          'top-[150px]',
          'w-full',
          'max-w-[189px]',
          'sm:hidden',
        ]"
        src="/images/airdrop/title_LikeCoin.png"
      />
      <div
        :class="[
          'bg-no-repeat',
          'z-[5]',
          'w-full',
          'h-full',
          'lg:bg-repeat',
        ]"
        :style="{
          backgroundImage: `url(/images/airdrop/background_cross.svg)`,
        }"
      >
        <div
          :class="[
            'z-[5]',
            'relative',
            'flex',
            'flex-col',
            'items-center',
            'justify-center',
            'mx-auto',
            'mt-[180px]',
            'mb-[32px]',
            'box-border',
            'bg-white',
            'sm:mt-[30%]',
            'lg:min-w-[936px]',
            'lg:max-w-[970px]',
            'lg:min-h-[300px]',
            'lg:mt-[300px]',
            '2xl:mt-[380px]',
          ]"
        >
          <!-- planet -->
          <img
            id="planet1"
            :class="[
              'hidden',
              'absolute',
              'top-[0px]',
              'translate-y-[-50%]',
              'right-[0]',
              'w-[30%]',
              'max-w-[340px]',
              'sm:block',
            ]"
            src="/images/airdrop/planet_1.png"
          />
          <img
            id="planet2"
            :class="[
              'hidden',
              'absolute',
              'top-[180px]',
              'left-[0]',
              'translate-x-[-80%]',
              'w-[30%]',
              'max-w-[276px]',
              'sm:block',
            ]"
            src="/images/airdrop/planet_2.png"
          />
          <div
            :class="[
              'overflow-hidden',
              'h-full',
              'w-full',
              'border-[2px]',
              'border-airdrop-gold',
              'rounded-[24px]',
            ]"
          >
            <nuxt />
          </div>
        </div>
        <!-- SubscriptionCard -->
        <SubscriptionCard
          v-if="!shouldShowSubscriptionCard"
          class="mb-[48px]"
          :preset="subscriptionCardPreset"
        />
        <div
          v-else
          :class="[
            'flex',
            'justify-center',
            'mb-[48px]',
          ]"
        >
          <Button
            preset="outline"
            :style="{ border: '2px solid #D1AB79', color: '#D1AB79' }"
            :text="$t('AirDrop.button.checker')"
            :to="localeLocation({ name: 'airdrop-check' })"
          >
            <template #prepend>
              <IconMissionButtonMini />
            </template>
          </Button>
        </div>
        <Label
          v-if="!!shouldShowTentative"
          :class="[
            'text-center',
            'mb-[120px]',
            'w-full',
            'max-w-[800px]',
            'mx-auto',
            'text-medium-gray',
            'sm:mb-[120px]',
          ]"
          :text="$t('AirDrop.content.tentative')"
          preset="p5"
        />
      </div>
    </div>
  </Page>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
// eslint-disable-next-line import/no-extraneous-dependencies
import { MetaInfo } from 'vue-meta'
import { namespace } from 'vuex-class'
import { AIRDROP_OG_ENDPOINT } from '~/constant'

const signerModule = namespace('signer')

@Component({
  head() {
    const title = this.$t('page.airdrop.title')
    return {
      title,
      meta: [
        {
          hid: 'og:image',
          property: 'og:image',
          content: `${AIRDROP_OG_ENDPOINT}airdrop_launch.png`,
        },
      ],
    } as MetaInfo
  },
})
export default class AirdropCheckPage extends Vue {
  @signerModule.Getter('getAddress') currentAddress!: string

  get shouldShowSubscriptionCard() {
    return this.$route.name === this.localeRoute({ name: 'airdrop' })?.name
  }

  get subscriptionCardPreset() {
    return this.$route.name ===
      this.localeRoute({ name: 'airdrop-check' })?.name
      ? 'community'
      : 'both'
  }

  get shouldShowTentative() {
    return (
      this.$route.name === this.localeRoute({ name: 'airdrop-check' })?.name
    )
  }
}
</script>
