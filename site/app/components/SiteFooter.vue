<script setup lang="ts">
import easyshootLogo from '~/assets/apps/easyshoot.svg'

const { text, lang } = useLang()
const { version, repo } = useRuntimeConfig().public

const home = computed(() => (lang.value === 'ar' ? '/ar/' : '/'))
const year = new Date().getFullYear()

interface FooterLink { label: string, to: string, external?: boolean, version?: boolean }

const columns = computed<{ title: string, links: FooterLink[] }[]>(() => [
  {
    title: text.value.footer.product,
    links: [
      { label: text.value.nav.features, to: `${home.value}#features` },
      { label: text.value.nav.screenshots, to: `${home.value}#screenshots` },
      { label: text.value.nav.install, to: `${home.value}#install` },
      { label: text.value.nav.faq, to: `${home.value}#faq` },
    ],
  },
  {
    title: text.value.nav.docs,
    links: Object.values(guides).map(guide => ({ label: guide[lang.value].nav, to: guide[lang.value].path })),
  },
  {
    title: text.value.footer.resources,
    links: [
      { label: 'GitHub', to: repo, external: true },
      { label: text.value.footer.releases, to: `${repo}/releases`, external: true, version: true },
      { label: text.value.footer.reportBug, to: `${repo}/issues/new/choose`, external: true },
    ],
  },
])

const siblingApps = computed(() => [
  { id: 'easyshoot', name: 'EasyShoot', url: 'https://easyshoot.app/', about: text.value.footer.apps.easyshoot },
  { id: 'pr-sift', name: 'PR Sift', url: 'https://pr-sift.haythamasalama.me/', about: text.value.footer.apps.prSift },
])
</script>

<template>
  <footer class="border-t border-default bg-default">
    <div class="mx-auto grid max-w-6xl grid-cols-2 gap-10 px-4 py-14 sm:px-6 md:grid-cols-[1.4fr_repeat(3,1fr)]">
      <div class="col-span-2 md:col-span-1">
        <a :href="home" class="flex items-center gap-2.5 font-semibold text-highlighted">
          <img :src="asset('logo.svg')" width="32" height="32" alt="" class="size-8 shrink-0">
          {{ text.brand }}
        </a>
        <p class="mt-4 max-w-60 text-sm leading-relaxed text-muted">
          {{ text.hero.title }}
        </p>
      </div>

      <div v-for="column in columns" :key="column.title">
        <p class="text-sm font-medium text-highlighted">
          {{ column.title }}
        </p>
        <ul class="mt-4 flex flex-col gap-2.5 text-sm text-muted">
          <li v-for="link in column.links" :key="link.to">
            <ULink raw :to="link.to" :target="link.external ? '_blank' : undefined" class="inline-flex items-center gap-2 hover:text-highlighted">
              {{ link.label }}
              <bdi v-if="link.version" dir="ltr" class="rounded-full border border-default px-2 font-mono text-xs">v{{ version }}</bdi>
            </ULink>
          </li>
        </ul>
      </div>
    </div>

    <div class="mx-auto max-w-6xl px-4 sm:px-6">
      <div class="flex flex-wrap items-center gap-x-6 gap-y-3 border-t border-default py-6">
        <p class="w-full text-sm text-muted sm:w-auto">
          {{ text.footer.siblingApps }}
        </p>
        <ul class="-mx-2.5 flex flex-wrap items-center gap-2 sm:mx-0">
          <li v-for="app in siblingApps" :key="app.id">
            <a :href="app.url" target="_blank" rel="noopener" class="flex items-center gap-3 rounded-xl py-2 ps-2.5 pe-4 transition-colors hover:bg-elevated">
              <img v-if="app.id === 'easyshoot'" :src="easyshootLogo" width="36" height="36" alt="" class="size-9 shrink-0">
              <PrSiftMark v-else class="size-9 shrink-0" />
              <span class="flex flex-col">
                <span class="font-semibold leading-tight tracking-tight text-highlighted">
                  <template v-if="app.id === 'easyshoot'">Easy<span class="text-[#1a63d6] dark:text-[#8cc4ff]">Shoot</span></template>
                  <template v-else>{{ app.name }}</template>
                </span>
                <span class="text-xs text-muted">{{ app.about }}</span>
              </span>
            </a>
          </li>
        </ul>
      </div>

      <div class="flex flex-wrap items-center justify-between gap-x-6 gap-y-4 border-t border-default py-6 text-sm">
        <p class="text-muted">
          {{ text.footer.copyright(year) }}
        </p>
        <nav :aria-label="text.footer.legal" class="-ms-3 flex items-center">
          <ULink raw :to="`${repo}/blob/main/PRIVACY.md`" target="_blank" class="px-3 leading-none text-toned hover:text-highlighted">
            {{ text.footer.privacy }}
          </ULink>
          <ULink raw :to="`${repo}/blob/main/LICENSE`" target="_blank" class="border-s border-default px-3 leading-none text-toned hover:text-highlighted">
            {{ text.footer.license }}
          </ULink>
          <ULink
            raw
            to="https://x.com/haythamasalama"
            target="_blank"
            :aria-label="text.footer.onX"
            title="X"
            class="ms-2 flex size-8 items-center justify-center rounded-full bg-elevated text-toned hover:text-highlighted"
          >
            <XMark class="size-3.5" />
          </ULink>
          <!-- His brand allows the signature only in pure black or pure white, so it skips the text tokens. -->
          <ULink raw to="https://haythamasalama.me/" target="_blank" aria-label="Haytham A. Salama" title="Haytham A. Salama" class="ms-4 text-black dark:text-white">
            <HaythamSignature class="h-7 w-14" />
          </ULink>
        </nav>
      </div>
    </div>
  </footer>
</template>
