<template>
  <div>
    <div class="container">
      <div class="p-4">
        <div
          v-if="currentUser === null"
          class="flex flex-col items-center justify-center"
        >
          <img
            :src="`/images/states/${$colorMode.value}/login.svg`"
            loading="lazy"
            class="inline-flex flex-col object-contain object-center w-24 h-24 my-4"
            :alt="`${t('empty.parameters')}`"
          />
          <p class="text-secondaryLight pb-4 text-center">
            {{ t("empty.profile") }}
          </p>
          <ButtonPrimary
            :label="t('auth.login')"
            class="mb-4"
            @click.native="showLogin = true"
          />
        </div>
        <div v-else class="space-y-8">
          <div
            class="bg-primaryLight md:h-32 -mb-11 h-24 rounded"
            style="background-image: url('/images/cover.svg')"
          ></div>
          <div class="md:flex-row flex flex-col justify-between px-4 space-y-8">
            <div class="flex items-end">
              <img
                v-if="currentUser.photoURL"
                :src="currentUser.photoURL"
                class="ring-4 ring-primary w-16 h-16 rounded-lg"
                :alt="currentUser.displayName"
              />
              <SmartIcon v-else name="user" class="svg-icons" />
              <div class="ml-4">
                <label class="heading">
                  {{ currentUser.displayName || t("state.nothing_found") }}
                </label>
                <p class="text-secondaryLight flex items-center">
                  {{ currentUser.email }}
                  <SmartIcon
                    v-if="currentUser.emailVerified"
                    name="verified"
                    class="svg-icons ml-2 text-green-500"
                  />
                </p>
              </div>
            </div>
            <div class="flex items-end space-x-2">
              <div>
                <SmartItem
                  to="/settings"
                  svg="settings"
                  :label="t('profile.app_settings')"
                  outline
                />
              </div>
              <FirebaseLogout outline />
            </div>
          </div>
          <SmartTabs>
            <SmartTab
              :id="'sync'"
              :label="t('settings.account')"
              :selected="true"
            >
              <section class="p-4">
                <h4 class="text-secondaryDark font-semibold">
                  {{ t("settings.profile") }}
                </h4>
                <div class="text-secondaryLight mt-1">
                  {{ t("settings.profile_description") }}
                </div>
                <div class="py-4">
                  <label for="displayName">
                    {{ t("settings.profile_name") }}
                  </label>
                  <form
                    class="md:max-w-sm flex mt-2"
                    @submit.prevent="updateDisplayName"
                  >
                    <input
                      id="displayName"
                      v-model="displayName"
                      class="input"
                      :placeholder="`${t('settings.profile_name')}`"
                      type="text"
                      autocomplete="off"
                      required
                    />
                    <ButtonPrimary
                      :label="t('action.save')"
                      class="min-w-16 ml-2"
                      type="submit"
                      :loading="updatingDisplayName"
                    />
                  </form>
                </div>
              </section>
              <section class="p-4">
                <h4 class="text-secondaryDark font-semibold">
                  {{ t("settings.sync") }}
                </h4>
                <div class="text-secondaryLight mt-1">
                  {{ t("settings.sync_description") }}
                </div>
                <div class="py-4 space-y-4">
                  <div class="flex items-center">
                    <SmartToggle
                      :on="SYNC_COLLECTIONS"
                      @change="toggleSetting('syncCollections')"
                    >
                      {{ t("settings.sync_collections") }}
                    </SmartToggle>
                  </div>
                  <div class="flex items-center">
                    <SmartToggle
                      :on="SYNC_ENVIRONMENTS"
                      @change="toggleSetting('syncEnvironments')"
                    >
                      {{ t("settings.sync_environments") }}
                    </SmartToggle>
                  </div>
                  <div class="flex items-center">
                    <SmartToggle
                      :on="SYNC_HISTORY"
                      @change="toggleSetting('syncHistory')"
                    >
                      {{ t("settings.sync_history") }}
                    </SmartToggle>
                  </div>
                </div>
              </section>
            </SmartTab>
            <SmartTab :id="'teams'" :label="t('team.title')">
              <AppSection label="teams">
                <Teams :modal="false" />
              </AppSection>
            </SmartTab>
          </SmartTabs>
        </div>
      </div>
      <FirebaseLogin :show="showLogin" @hide-modal="showLogin = false" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, useMeta, defineComponent } from "@nuxtjs/composition-api"
import { currentUser$, setDisplayName } from "~/helpers/fb/auth"
import { useReadonlyStream, useI18n } from "~/helpers/utils/composables"
import { toggleSetting, useSetting } from "~/newstore/settings"

const t = useI18n()

const showLogin = ref(false)

const SYNC_COLLECTIONS = useSetting("syncCollections")
const SYNC_ENVIRONMENTS = useSetting("syncEnvironments")
const SYNC_HISTORY = useSetting("syncHistory")
const currentUser = useReadonlyStream(currentUser$, null)

const displayName = ref(currentUser$.value?.displayName)
const updatingDisplayName = ref(false)

const updateDisplayName = () => {
  updatingDisplayName.value = true
  setDisplayName(displayName.value).finally(() => {
    updatingDisplayName.value = false
  })
}

useMeta({
  title: `${t("navigation.profile")} • Hoppscotch`,
})
</script>

<script lang="ts">
export default defineComponent({
  head: {},
})
</script>
