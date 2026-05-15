<template>
  <!-- Custom Home Content: Full Page Mode -->
  <div v-if="homeContent" class="min-h-screen">
    <!-- iframe mode -->
    <iframe
      v-if="isHomeContentUrl"
      :src="homeContent.trim()"
      class="h-screen w-full border-0"
      allowfullscreen
    ></iframe>
    <!-- HTML mode - SECURITY: homeContent is admin-only setting, XSS risk is acceptable -->
    <div v-else v-html="homeContent"></div>
  </div>

  <!-- Default Home Page -->
  <div
    v-else
    class="relayai-page antialiased text-slate-900 dark:text-slate-100 bg-white dark:bg-slate-950"
  >
    <header
      id="site-nav"
      class="fixed inset-x-0 top-0 z-50 border-b border-slate-200 bg-white/95 backdrop-blur transition-shadow dark:border-slate-800 dark:bg-slate-950/85"
      :class="{ 'nav-scrolled': isScrolled }"
    >
      <div
        class="mx-auto flex h-16 max-w-7xl items-center justify-between px-6"
      >
        <a href="#top" class="group flex items-center gap-2.5">
          <span class="brand-mark" aria-hidden="true">
            <svg viewBox="0 0 24 24" width="22" height="22">
              <rect
                x="2.5"
                y="11"
                width="1.6"
                height="2"
                rx="0.8"
                class="sf-tail"
                opacity="0.32"
              />
              <rect
                x="5.5"
                y="11"
                width="2.6"
                height="2"
                rx="1"
                class="sf-tail"
                opacity="0.6"
              />
              <rect
                x="9.5"
                y="11"
                width="3.6"
                height="2"
                rx="1"
                class="sf-tail"
              />
              <path d="M15.5 5 L23 12 L15.5 19 Z" class="sf-main" />
              <path d="M14 9 L17.5 12 L14 15 Z" fill="#0D9488" />
            </svg>
          </span>
          <span class="wordmark text-[18px]" aria-label="RelayAI">
            <span class="wm-relay">Relay</span><span class="wm-ai">AI</span>
          </span>
        </a>

        <nav
          class="hidden items-center gap-8 text-[14px] text-slate-600 dark:text-slate-400 md:flex"
        >
          <a
            href="#models"
            class="transition-colors hover:text-slate-900 dark:hover:text-white"
            >模型</a
          >
          <a
            href="#pricing"
            class="transition-colors hover:text-slate-900 dark:hover:text-white"
            >定价</a
          >
          <a
            v-if="docUrl"
            :href="docUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="transition-colors hover:text-slate-900 dark:hover:text-white"
            >文档</a
          >
          <a
            v-else
            href="#docs"
            class="transition-colors hover:text-slate-900 dark:hover:text-white"
            >接入</a
          >
          <a
            href="#faq"
            class="transition-colors hover:text-slate-900 dark:hover:text-white"
            >常见问题</a
          >
        </nav>

        <div class="flex items-center gap-1.5">
          <button
            type="button"
            class="rounded-lg p-2 text-slate-500 transition-colors hover:bg-slate-100 hover:text-slate-900 dark:text-slate-400 dark:hover:bg-slate-800 dark:hover:text-white"
            :aria-label="isDark ? '切换到浅色模式' : '切换到深色模式'"
            @click="toggleTheme"
          >
            <svg
              v-if="isDark"
              width="18"
              height="18"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <circle cx="12" cy="12" r="4" />
              <path d="M12 2v2" />
              <path d="M12 20v2" />
              <path d="m4.93 4.93 1.41 1.41" />
              <path d="m17.66 17.66 1.41 1.41" />
              <path d="M2 12h2" />
              <path d="M20 12h2" />
              <path d="m6.34 17.66-1.41 1.41" />
              <path d="m19.07 4.93-1.41 1.41" />
            </svg>
            <svg
              v-else
              width="18"
              height="18"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z" />
            </svg>
          </button>

          <template v-if="isAuthenticated">
            <router-link
              :to="dashboardPath"
              class="brand-cta inline-flex items-center gap-1.5 rounded-lg px-3.5 py-2 text-[14px] font-medium text-white shadow-sm transition-colors"
            >
              控制台
              <svg
                width="14"
                height="14"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2.2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path d="M5 12h14" />
                <path d="m12 5 7 7-7 7" />
              </svg>
            </router-link>
          </template>
          <template v-else>
            <router-link
              to="/login"
              class="hidden rounded-lg px-3 py-2 text-[14px] font-medium text-slate-700 transition-colors hover:text-slate-900 sm:inline-flex dark:text-slate-300 dark:hover:text-white"
              >登录</router-link
            >
            <router-link
              to="/register"
              class="brand-cta inline-flex items-center rounded-lg px-3.5 py-2 text-[14px] font-medium text-white shadow-sm transition-colors"
              >注册</router-link
            >
          </template>
          <button
            type="button"
            class="-mr-2 p-2 text-slate-700 dark:text-slate-300 md:hidden"
            aria-label="打开菜单"
            @click="mobileMenuOpen = !mobileMenuOpen"
          >
            <svg
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <line x1="4" x2="20" y1="6" y2="6" />
              <line x1="4" x2="20" y1="12" y2="12" />
              <line x1="4" x2="20" y1="18" y2="18" />
            </svg>
          </button>
        </div>
      </div>

      <div
        v-if="mobileMenuOpen"
        class="border-t border-slate-200 bg-white dark:border-slate-800 dark:bg-slate-950 md:hidden"
      >
        <div class="flex flex-col gap-1 px-6 py-4 text-[15px]">
          <a
            href="#models"
            class="py-2 text-slate-700 dark:text-slate-300"
            @click="mobileMenuOpen = false"
            >模型</a
          >
          <a
            href="#pricing"
            class="py-2 text-slate-700 dark:text-slate-300"
            @click="mobileMenuOpen = false"
            >定价</a
          >
          <a
            v-if="docUrl"
            :href="docUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="py-2 text-slate-700 dark:text-slate-300"
            @click="mobileMenuOpen = false"
            >文档</a
          >
          <a
            v-else
            href="#docs"
            class="py-2 text-slate-700 dark:text-slate-300"
            @click="mobileMenuOpen = false"
            >接入</a
          >
          <a
            href="#faq"
            class="py-2 text-slate-700 dark:text-slate-300"
            @click="mobileMenuOpen = false"
            >常见问题</a
          >
          <router-link
            v-if="!isAuthenticated"
            to="/login"
            class="py-2 text-slate-700 dark:text-slate-300"
            >登录</router-link
          >
        </div>
      </div>
    </header>

    <!-- ============ HERO ============ -->
    <section
      id="top"
      class="hero-bg border-b border-slate-100 pb-20 pt-32 dark:border-slate-800 md:pb-28 md:pt-40"
    >
      <div class="mx-auto max-w-7xl px-6">
        <div class="grid items-center gap-12 lg:grid-cols-2 lg:gap-16">
          <div class="reveal">
            <div
              class="brand-chip mb-6 inline-flex items-center gap-2 rounded-full px-3 py-1 text-[12px] font-medium"
            >
              <span class="brand-chip-dot"></span>
              OpenAI 协议兼容 · 三分钟接入
            </div>
            <h1
              class="font-cn text-[40px] font-bold leading-[1.1] tracking-tight text-slate-900 dark:text-white sm:text-[44px] lg:text-[48px] xl:text-[56px]"
            >
              <span class="block">更稳、更近、更可控的</span>
              <span class="block">AI 模型直连</span>
            </h1>
            <p
              class="mt-4 text-[18px] font-normal italic text-slate-500 dark:text-slate-400 md:text-[20px]"
            >
              The Relay for Every AI Call
            </p>
            <p
              class="font-cn mt-6 max-w-xl text-[15.5px] leading-[1.75] text-slate-600 dark:text-slate-300 md:text-[16.5px]"
            >
              目前已接入 OpenAI 全系（GPT-5.5、GPT-5.5
              mini、Codex），Claude、Gemini 即将上线。完整兼容 OpenAI
              协议——开发者工具（Codex
              CLI、Cursor、Cline）与图形客户端（Chatbox、NextChat、OpenCat
              等）均可使用，仅需替换 base_url。
            </p>

            <div class="mt-8 flex flex-wrap gap-3">
              <router-link
                :to="primaryCtaPath"
                class="brand-cta inline-flex items-center gap-2 rounded-lg px-5 py-3 text-[15px] font-medium text-white shadow-sm transition-all hover:-translate-y-0.5 hover:shadow-md"
              >
                {{ primaryCtaLabel }}
                <svg
                  width="16"
                  height="16"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="2.2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                >
                  <path d="M5 12h14" />
                  <path d="m12 5 7 7-7 7" />
                </svg>
              </router-link>
              <a
                :href="docUrl || '#docs'"
                :target="docUrl ? '_blank' : '_self'"
                :rel="docUrl ? 'noopener noreferrer' : undefined"
                class="brand-ghost inline-flex items-center gap-2 rounded-lg border bg-white px-5 py-3 text-[15px] font-medium transition-colors dark:bg-transparent"
              >
                查看文档
              </a>
            </div>

            <p
              class="font-cn mt-5 flex flex-wrap items-center gap-x-2 gap-y-1 text-[13.5px] text-slate-500 dark:text-slate-400"
            >
              <span>按量计费</span>
              <span class="text-slate-300 dark:text-slate-600">·</span>
              <span>无月费</span>
              <span class="text-slate-300 dark:text-slate-600">·</span>
              <span>无最低消费</span>
              <span class="text-slate-300 dark:text-slate-600">·</span>
              <span>余额可退</span>
            </p>
          </div>

          <div class="reveal" style="transition-delay: 80ms">
            <div class="relative">
              <div class="absolute -right-2 -top-3 z-10 rotate-2 md:-right-4">
                <div
                  class="font-cn inline-flex items-center gap-1.5 rounded-full border border-slate-200 bg-white px-3 py-1.5 text-[12px] font-medium text-slate-700 shadow-card dark:border-slate-700 dark:bg-slate-900 dark:text-slate-200"
                >
                  <span
                    class="h-1.5 w-1.5 animate-pulse rounded-full bg-emerald-500"
                  ></span>
                  替换 base_url 即可使用
                </div>
              </div>

              <div
                class="overflow-hidden rounded-xl border border-slate-800 bg-slate-900 shadow-2xl shadow-slate-900/20"
              >
                <div
                  class="flex items-center justify-between border-b border-slate-800 bg-slate-900/80 px-4 py-3"
                >
                  <div class="flex items-center gap-1.5">
                    <span class="h-3 w-3 rounded-full bg-slate-700"></span>
                    <span class="h-3 w-3 rounded-full bg-slate-700"></span>
                    <span class="h-3 w-3 rounded-full bg-slate-700"></span>
                  </div>
                  <span class="font-mono text-[12px] text-slate-500"
                    >example.py</span
                  >
                  <span class="font-mono text-[11px] text-slate-600"
                    >Python</span
                  >
                </div>
                <pre
                  class="overflow-x-auto p-5 font-mono text-[13.5px] leading-[1.7]"
                ><code><span class="tok-kw">from</span> <span class="tok-var">openai</span> <span class="tok-kw">import</span> <span class="tok-fn">OpenAI</span>

<span class="tok-var">client</span> <span class="tok-punc">=</span> <span class="tok-fn">OpenAI</span><span class="tok-punc">(</span>
    <span class="tok-var">base_url</span><span class="tok-punc">=</span><span class="tok-str">"https://relayai.asia/v1"</span><span class="tok-punc">,</span>
    <span class="tok-var">api_key</span><span class="tok-punc">=</span><span class="tok-str">"sk-xxxxx"</span>
<span class="tok-punc">)</span>

<span class="tok-var">response</span> <span class="tok-punc">=</span> <span class="tok-var">client</span><span class="tok-punc">.</span><span class="tok-var">chat</span><span class="tok-punc">.</span><span class="tok-var">completions</span><span class="tok-punc">.</span><span class="tok-fn">create</span><span class="tok-punc">(</span>
    <span class="tok-var">model</span><span class="tok-punc">=</span><span class="tok-str">"gpt-5.5"</span><span class="tok-punc">,</span>
    <span class="tok-var">messages</span><span class="tok-punc">=[{</span><span class="tok-str">"role"</span><span class="tok-punc">:</span> <span class="tok-str">"user"</span><span class="tok-punc">,</span> <span class="tok-str">"content"</span><span class="tok-punc">:</span> <span class="tok-str">"Hello"</span><span class="tok-punc">}]</span>
<span class="tok-punc">)</span></code></pre>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ MODELS ============ -->
    <section
      id="models"
      class="border-b border-slate-100 bg-white py-20 dark:border-slate-800 dark:bg-slate-950 md:py-28"
    >
      <div class="mx-auto max-w-7xl px-6">
        <div class="reveal max-w-2xl">
          <h2
            class="font-cn text-[30px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[36px]"
          >
            覆盖主流大模型，统一接口调用
          </h2>
          <p
            class="font-cn mt-4 text-[16px] leading-relaxed text-slate-600 dark:text-slate-300 md:text-[17px]"
          >
            一个 API Key，调用 OpenAI、Anthropic、Google
            的旗舰模型，无需多账号、多支付。
          </p>
        </div>

        <div class="mt-12 grid gap-4 sm:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="(model, idx) in models"
            :key="model.name"
            class="lift reveal rounded-xl border border-slate-200 bg-white p-6 dark:border-slate-800 dark:bg-slate-900"
            :style="{ transitionDelay: `${idx * 60}ms` }"
          >
            <div class="flex items-start justify-between">
              <div
                class="flex h-10 w-10 items-center justify-center rounded-lg bg-slate-900 text-white dark:bg-slate-800"
              >
                <svg
                  width="20"
                  height="20"
                  viewBox="0 0 24 24"
                  fill="none"
                  stroke="currentColor"
                  stroke-width="1.8"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  v-html="model.iconPath"
                />
              </div>
              <span
                class="inline-flex items-center gap-1.5 rounded-full border border-emerald-100 bg-emerald-50 px-2 py-0.5 text-[11px] font-medium text-emerald-700 dark:border-emerald-700/60 dark:bg-emerald-900/30 dark:text-emerald-300"
              >
                <span class="h-1.5 w-1.5 rounded-full bg-emerald-500"></span>
                Live
              </span>
            </div>
            <div
              class="mt-4 text-[12px] font-medium uppercase tracking-wide text-slate-500 dark:text-slate-400"
            >
              {{ model.vendor }}
            </div>
            <h3
              class="mt-1 text-[18px] font-semibold text-slate-900 dark:text-white"
            >
              {{ model.name }}
              <span
                v-if="model.subName"
                class="text-[14px] font-normal text-slate-400 dark:text-slate-500"
              >
                ({{ model.subName }})
              </span>
            </h3>
            <p
              class="font-cn mt-2 text-[14px] leading-relaxed text-slate-600 dark:text-slate-300"
            >
              {{ model.desc }}
            </p>
          </article>
        </div>

        <div class="reveal mt-4">
          <div
            class="flex flex-col gap-4 rounded-xl border border-slate-200 bg-slate-50 px-6 py-5 dark:border-slate-800 dark:bg-slate-900/60 sm:flex-row sm:items-center sm:gap-5"
          >
            <div class="flex flex-shrink-0 items-center gap-4">
              <div
                class="flex items-center gap-3"
                aria-label="Claude 与 Gemini"
              >
                <svg
                  width="22"
                  height="22"
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                  class="text-slate-400 dark:text-slate-500"
                >
                  <title>Claude</title>
                  <circle
                    cx="12"
                    cy="12"
                    r="9"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="1.6"
                  />
                  <path
                    d="M8.2 14.8c1-2 1.6-4.2 1.6-6.4M15.8 14.8c-1-2-1.6-4.2-1.6-6.4M7.5 12.4h9"
                    stroke="currentColor"
                    stroke-width="1.6"
                    stroke-linecap="round"
                    fill="none"
                  />
                </svg>
                <svg
                  width="22"
                  height="22"
                  viewBox="0 0 24 24"
                  aria-hidden="true"
                  class="text-slate-400 dark:text-slate-500"
                >
                  <title>Gemini</title>
                  <path
                    d="M12 2c.3 4.2 2.2 7.5 6.5 8.5C18.2 11 14.5 11.7 12 15c-.2-4-2.3-7.5-6.5-8.5C9.5 6 11.7 5.3 12 2z"
                    fill="currentColor"
                    opacity="0.65"
                  />
                  <path
                    d="M12 22c-.3-4.2-2.2-7.5-6.5-8.5C5.8 13 9.5 12.3 12 9c.2 4 2.3 7.5 6.5 8.5C14.5 18 12.3 18.7 12 22z"
                    fill="currentColor"
                  />
                </svg>
              </div>
              <span
                class="font-cn inline-flex items-center gap-2 whitespace-nowrap text-[13px] font-semibold text-slate-900 dark:text-white"
              >
                <span class="h-1.5 w-1.5 rounded-full bg-amber-400"></span
                >即将接入
              </span>
            </div>
            <div
              class="hidden h-8 w-px bg-slate-200 dark:bg-slate-700 sm:block"
            ></div>
            <p
              class="font-cn text-[14px] leading-[1.7] text-slate-600 dark:text-slate-300"
            >
              <span class="font-medium text-slate-700 dark:text-slate-200"
                >Anthropic Claude</span
              >（Opus 4.7 / Sonnet 4.6）、<span
                class="font-medium text-slate-700 dark:text-slate-200"
                >Google Gemini 3 Pro</span
              >
              等模型陆续上线，关注我们获取最新进展。
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ FEATURES ============ -->
    <section
      class="border-b border-slate-100 bg-slate-50/60 py-20 dark:border-slate-800 dark:bg-slate-900/40 md:py-28"
    >
      <div class="mx-auto max-w-7xl px-6">
        <div class="reveal max-w-2xl">
          <h2
            class="font-cn text-[30px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[36px]"
          >
            为什么选择
            <span class="wordmark">
              <span class="wm-relay">Relay</span><span class="wm-ai">AI</span>
            </span>
          </h2>
          <p
            class="font-cn mt-4 text-[16px] leading-relaxed text-slate-600 dark:text-slate-300 md:text-[17px]"
          >
            我们解决的，是接入海外模型时真正会遇到的问题。
          </p>
        </div>

        <div class="mt-12 grid gap-4 md:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="(feat, idx) in features"
            :key="feat.title"
            class="lift reveal rounded-2xl border border-slate-200 bg-white p-7 dark:border-slate-800 dark:bg-slate-900"
            :style="{ transitionDelay: `${(idx % 2) * 60}ms` }"
          >
            <div class="feat-icon">
              <svg
                width="22"
                height="22"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="1.8"
                stroke-linecap="round"
                stroke-linejoin="round"
                v-html="feat.iconPath"
              />
            </div>
            <h3
              class="font-cn mt-5 text-[18px] font-semibold text-slate-900 dark:text-white"
            >
              {{ feat.title }}
            </h3>
            <p
              class="font-cn mt-2 text-[14.5px] leading-[1.7] text-slate-600 dark:text-slate-300"
            >
              {{ feat.desc }}
            </p>
          </article>
        </div>
      </div>
    </section>

    <!-- ============ PRICING ============ -->
    <section
      id="pricing"
      class="border-b border-slate-100 bg-white py-20 dark:border-slate-800 dark:bg-slate-950 md:py-28"
    >
      <div class="mx-auto max-w-6xl px-6">
        <div class="reveal max-w-2xl">
          <h2
            class="font-cn text-[30px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[36px]"
          >
            透明定价，按真实用量计费
          </h2>
          <p
            class="font-cn mt-4 text-[16px] leading-relaxed text-slate-600 dark:text-slate-300 md:text-[17px]"
          >
            按真实 token 用量计费，<span class="font-mono text-[15px]"
              >1 RMB = $1</span
            >
            平台积分。不同模型的倍率根据上游成本独立定价，具体见下表。
          </p>
        </div>

        <div class="pricing-grid mt-10 grid gap-4 md:grid-cols-3 md:gap-5">
          <article
            v-for="(price, idx) in pricing"
            :key="price.name"
            class="price-card reveal flex flex-col rounded-2xl bg-white p-7 dark:bg-slate-900"
            :class="{ 'price-card-featured': price.recommended }"
            :style="{ transitionDelay: `${idx * 60}ms` }"
          >
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-2">
                <span
                  class="text-[12px] font-medium uppercase tracking-wide text-slate-500 dark:text-slate-400"
                >
                  {{ price.vendor }}
                </span>
                <span
                  v-if="price.recommended"
                  class="font-cn brand-cta inline-flex items-center rounded-full px-2 py-0.5 text-[10.5px] font-semibold tracking-wide text-white"
                  >推荐</span
                >
              </div>
              <span
                class="inline-flex items-center gap-1.5 rounded-full border border-emerald-100 bg-emerald-50 px-2 py-0.5 text-[11px] font-medium text-emerald-700 dark:border-emerald-700/60 dark:bg-emerald-900/30 dark:text-emerald-300"
              >
                <span class="h-1.5 w-1.5 rounded-full bg-emerald-500"></span
                >Live
              </span>
            </div>
            <h3
              class="mt-3 text-[22px] font-bold tracking-tight text-slate-900 dark:text-white"
            >
              {{ price.name }}
              <span
                v-if="price.subName"
                class="align-middle text-[15px] font-normal text-slate-400 dark:text-slate-500"
              >
                {{ price.subName }}
              </span>
            </h3>
            <p
              class="font-cn mt-2 min-h-[44px] text-[13.5px] leading-[1.6] text-slate-500 dark:text-slate-400"
            >
              {{ price.desc }}
            </p>
            <div
              class="mt-6 space-y-2.5 border-t border-slate-100 pt-5 dark:border-slate-800"
            >
              <div class="flex items-baseline justify-between">
                <span
                  class="font-cn text-[12.5px] text-slate-500 dark:text-slate-400"
                  >输入</span
                >
                <span
                  class="font-mono text-[15px] text-slate-900 dark:text-slate-100"
                >
                  <span
                    class="font-semibold"
                    :class="{
                      'text-primary-600 dark:text-primary-300':
                        price.recommended,
                    }"
                  >
                    ¥{{ price.input }}
                  </span>
                  <span
                    class="text-[12.5px] text-slate-400 dark:text-slate-500"
                  >
                    / MTok</span
                  >
                </span>
              </div>
              <div class="flex items-baseline justify-between">
                <span
                  class="font-cn text-[12.5px] text-slate-500 dark:text-slate-400"
                  >输出</span
                >
                <span
                  class="font-mono text-[15px] text-slate-900 dark:text-slate-100"
                >
                  <span
                    class="font-semibold"
                    :class="{
                      'text-primary-600 dark:text-primary-300':
                        price.recommended,
                    }"
                  >
                    ¥{{ price.output }}
                  </span>
                  <span
                    class="text-[12.5px] text-slate-400 dark:text-slate-500"
                  >
                    / MTok</span
                  >
                </span>
              </div>
            </div>
            <p
              class="font-cn mt-6 text-[12.5px] text-slate-400 dark:text-slate-500"
            >
              即时计费 · 无月费 · 无最低消费
            </p>
          </article>
        </div>

        <p
          class="reveal font-cn mt-5 text-[12.5px] leading-relaxed text-slate-500 dark:text-slate-400"
        >
          Claude、Gemini
          等模型的定价将在正式上线时公布，会根据上游成本独立设定，不承诺统一倍率。
        </p>

        <div class="reveal mt-6">
          <div
            class="font-cn pricing-tip flex items-start gap-3 rounded-xl p-4 text-[14px] leading-[1.75]"
          >
            <svg
              class="mt-0.5 flex-shrink-0 text-primary-600 dark:text-primary-300"
              width="18"
              height="18"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path
                d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A6 6 0 0 0 6 8c0 1 .2 2.2 1.5 3.5.7.7 1.3 1.5 1.5 2.5"
              />
              <path d="M9 18h6" />
              <path d="M10 22h4" />
            </svg>
            <p class="text-slate-700 dark:text-slate-200">
              充值无门槛，最低 <span class="font-mono">¥10</span> 起；余额永不过期，未消费部分可申请退款。<br />
              价格会随上游成本变化及时调整。
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ STEPS ============ -->
    <section
      id="docs"
      class="border-b border-slate-100 bg-slate-50/60 py-20 dark:border-slate-800 dark:bg-slate-900/40 md:py-28"
    >
      <div class="mx-auto max-w-7xl px-6">
        <div class="reveal max-w-2xl">
          <h2
            class="font-cn text-[30px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[36px]"
          >
            三步开始使用
          </h2>
          <p
            class="font-cn mt-4 text-[16px] leading-relaxed text-slate-600 dark:text-slate-300 md:text-[17px]"
          >
            从注册到第一次成功调用，通常不超过三分钟。
          </p>
        </div>

        <div class="relative mt-14">
          <div
            class="step-connector absolute left-[16%] right-[16%] top-7 hidden md:block"
          ></div>

          <div class="relative grid gap-10 md:grid-cols-3 md:gap-6">
            <div class="reveal text-center">
              <div class="step-num relative z-10 mx-auto">01</div>
              <h3
                class="font-cn mt-5 text-[18px] font-semibold text-slate-900 dark:text-white"
              >
                注册账号
              </h3>
              <p
                class="font-cn mx-auto mt-2 max-w-[280px] text-[14.5px] leading-[1.7] text-slate-600 dark:text-slate-300"
              >
                邮箱注册，三十秒完成，无需手机号验证。
              </p>
            </div>
            <div class="reveal text-center" style="transition-delay: 80ms">
              <div class="step-num relative z-10 mx-auto">02</div>
              <h3
                class="font-cn mt-5 text-[18px] font-semibold text-slate-900 dark:text-white"
              >
                充值与生成 API Key
              </h3>
              <p
                class="font-cn mx-auto mt-2 max-w-[280px] text-[14.5px] leading-[1.7] text-slate-600 dark:text-slate-300"
              >
                扫码充值人民币，后台一键生成 API Key。
              </p>
            </div>
            <div class="reveal text-center" style="transition-delay: 160ms">
              <div class="step-num relative z-10 mx-auto">03</div>
              <h3
                class="font-cn mt-5 text-[18px] font-semibold text-slate-900 dark:text-white"
              >
                替换 base_url
              </h3>
              <p
                class="font-cn mx-auto mt-2 max-w-[300px] text-[14.5px] leading-[1.7] text-slate-600 dark:text-slate-300"
              >
                将你的客户端 base_url 改为
              </p>
              <code
                class="mt-2 inline-block rounded-md bg-slate-900 px-3 py-1.5 font-mono text-[12.5px] text-primary-200 dark:bg-slate-800"
              >
                {{ apiBaseUrl }}
              </code>
              <p
                class="font-cn mt-2 text-[14.5px] text-slate-600 dark:text-slate-300"
              >
                代码无需改动。
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ============ FAQ ============ -->
    <section
      id="faq"
      class="border-b border-slate-100 bg-white py-20 dark:border-slate-800 dark:bg-slate-950 md:py-28"
    >
      <div class="mx-auto max-w-3xl px-6">
        <div class="reveal">
          <h2
            class="font-cn text-[30px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[36px]"
          >
            常见问题
          </h2>
          <p
            class="font-cn mt-4 text-[15px] text-slate-600 dark:text-slate-300"
          >
            还有其他问题？联系
            <a
              :href="`mailto:${supportEmail}`"
              class="font-mono text-primary-600 hover:underline dark:text-primary-300"
            >
              {{ supportEmail }}
            </a>
          </p>
        </div>

        <div
          class="reveal mt-10 divide-y divide-slate-200 border-y border-slate-200 dark:divide-slate-800 dark:border-slate-800"
        >
          <details v-for="qa in faqs" :key="qa.q" class="group py-5">
            <summary
              class="font-cn flex items-start justify-between gap-4 text-[16px] font-medium text-slate-900 dark:text-white"
            >
              <span v-html="qa.q" />
              <svg
                class="faq-chevron mt-1 flex-shrink-0 text-slate-400 dark:text-slate-500"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <polyline points="6 9 12 15 18 9" />
              </svg>
            </summary>
            <p
              class="font-cn mt-3 text-[14.5px] leading-[1.75] text-slate-600 dark:text-slate-300"
              v-html="qa.a"
            />
          </details>
        </div>
      </div>
    </section>

    <!-- ============ CTA ============ -->
    <section
      class="cta-band border-b border-slate-100 py-20 dark:border-slate-800 md:py-24"
    >
      <div class="reveal mx-auto max-w-3xl px-6 text-center">
        <h2
          class="font-cn text-[28px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[36px]"
        >
          开始你的 AI 集成
        </h2>
        <p
          class="font-cn mt-4 text-[16px] text-slate-600 dark:text-slate-300 md:text-[17px]"
        >
          OpenAI 协议兼容，无需海外信用卡，三分钟接入到你的开发工具。
        </p>
        <div class="mt-7">
          <router-link
            :to="primaryCtaPath"
            class="brand-cta inline-flex items-center gap-2 rounded-lg px-6 py-3.5 text-[15px] font-medium text-white shadow-sm transition-all hover:-translate-y-0.5 hover:shadow-md"
          >
            {{ primaryCtaLabel }}
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2.2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path d="M5 12h14" />
              <path d="m12 5 7 7-7 7" />
            </svg>
          </router-link>
        </div>
      </div>
    </section>

    <!-- ============ FOOTER ============ -->
    <footer class="bg-white dark:bg-slate-950">
      <div class="mx-auto max-w-7xl px-6 py-14">
        <div class="grid grid-cols-2 gap-10 md:grid-cols-4">
          <div class="col-span-2 md:col-span-1">
            <div class="flex items-center gap-2">
              <span class="brand-mark" aria-hidden="true">
                <svg viewBox="0 0 24 24" width="22" height="22">
                  <rect
                    x="2.5"
                    y="11"
                    width="1.6"
                    height="2"
                    rx="0.8"
                    class="sf-tail"
                    opacity="0.32"
                  />
                  <rect
                    x="5.5"
                    y="11"
                    width="2.6"
                    height="2"
                    rx="1"
                    class="sf-tail"
                    opacity="0.6"
                  />
                  <rect
                    x="9.5"
                    y="11"
                    width="3.6"
                    height="2"
                    rx="1"
                    class="sf-tail"
                  />
                  <path d="M15.5 5 L23 12 L15.5 19 Z" class="sf-main" />
                  <path d="M14 9 L17.5 12 L14 15 Z" fill="#0D9488" />
                </svg>
              </span>
              <span class="wordmark text-[17px]" aria-label="RelayAI">
                <span class="wm-relay">Relay</span><span class="wm-ai">AI</span>
              </span>
            </div>
            <p
              class="font-cn mt-4 max-w-[240px] text-[13.5px] leading-relaxed text-slate-500 dark:text-slate-400"
            >
              稳定接入主流 AI 模型的中转服务。
            </p>
            <a
              :href="`mailto:${supportEmail}`"
              class="mt-4 inline-flex items-center gap-1.5 font-mono text-[13.5px] text-slate-600 hover:text-primary-600 dark:text-slate-400 dark:hover:text-primary-300"
            >
              {{ supportEmail }}
            </a>
          </div>

          <div>
            <div
              class="font-cn text-[12px] font-semibold uppercase tracking-wider text-slate-900 dark:text-white"
            >
              产品
            </div>
            <ul
              class="mt-4 space-y-3 text-[14px] text-slate-600 dark:text-slate-400"
            >
              <li>
                <a
                  href="#pricing"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >定价</a
                >
              </li>
              <li>
                <a
                  href="#models"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >模型支持</a
                >
              </li>
              <li>
                <a
                  v-if="docUrl"
                  :href="docUrl"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >接入文档</a
                >
                <a
                  v-else
                  href="#docs"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >接入文档</a
                >
              </li>
            </ul>
          </div>

          <div>
            <div
              class="font-cn text-[12px] font-semibold uppercase tracking-wider text-slate-900 dark:text-white"
            >
              资源
            </div>
            <ul
              class="mt-4 space-y-3 text-[14px] text-slate-600 dark:text-slate-400"
            >
              <li>
                <a
                  href="#faq"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >常见问题</a
                >
              </li>
              <li>
                <a
                  href="#docs"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >接入示例</a
                >
              </li>
            </ul>
          </div>

          <div v-if="legalDocuments.length">
            <div
              class="font-cn text-[12px] font-semibold uppercase tracking-wider text-slate-900 dark:text-white"
            >
              法律
            </div>
            <ul
              class="mt-4 space-y-3 text-[14px] text-slate-600 dark:text-slate-400"
            >
              <li v-for="doc in legalDocuments" :key="doc.id">
                <router-link
                  :to="`/legal/${doc.id}`"
                  class="transition-colors hover:text-slate-900 dark:hover:text-white"
                  >{{ doc.title }}</router-link
                >
              </li>
            </ul>
          </div>
        </div>

        <div
          class="mt-12 flex flex-col items-start justify-between gap-3 border-t border-slate-200 pt-6 dark:border-slate-800 sm:flex-row sm:items-center"
        >
          <p class="text-[13px] text-slate-500 dark:text-slate-400">
            © {{ currentYear }}
            <span class="wordmark">
              <span class="wm-relay">Relay</span
              ><span class="wm-ai">AI</span> </span
            >. All rights reserved.
          </p>
          <p class="font-mono text-[12.5px] text-slate-400 dark:text-slate-500">
            {{ domain }}
          </p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue';
import { useAuthStore, useAppStore } from '@/stores';

const authStore = useAuthStore();
const appStore = useAppStore();

const homeContent = computed(
  () => appStore.cachedPublicSettings?.home_content || '',
);
const isHomeContentUrl = computed(() => {
  const content = homeContent.value.trim();
  return content.startsWith('http://') || content.startsWith('https://');
});

const docUrl = computed(
  () => appStore.cachedPublicSettings?.doc_url || appStore.docUrl || '',
);

const legalDocuments = computed(
  () => appStore.cachedPublicSettings?.login_agreement_documents ?? [],
);

const isAuthenticated = computed(() => authStore.isAuthenticated);
const isAdmin = computed(() => authStore.isAdmin);
const dashboardPath = computed(() =>
  isAdmin.value ? '/admin/dashboard' : '/dashboard',
);

const primaryCtaPath = computed(() =>
  isAuthenticated.value ? dashboardPath.value : '/register',
);
const primaryCtaLabel = computed(() =>
  isAuthenticated.value ? '进入控制台' : '立即开始',
);

const currentYear = computed(() => new Date().getFullYear());
const domain = 'relayai.asia';
const supportEmail = `support@${domain}`;
const apiBaseUrl = `https://${domain}/v1`;

const mobileMenuOpen = ref(false);
const isScrolled = ref(false);
const isDark = ref(false);

function applyTheme(dark: boolean) {
  isDark.value = dark;
  document.documentElement.classList.toggle('dark', dark);
  localStorage.setItem('theme', dark ? 'dark' : 'light');
}

function toggleTheme() {
  applyTheme(!isDark.value);
}

function initTheme() {
  const saved = localStorage.getItem('theme');
  if (saved === 'dark' || saved === 'light') {
    applyTheme(saved === 'dark');
    return;
  }
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  isDark.value =
    document.documentElement.classList.contains('dark') || prefersDark;
  document.documentElement.classList.toggle('dark', isDark.value);
}

const models = [
  {
    vendor: 'OpenAI',
    name: 'GPT-5.5 mini',
    desc: '性价比之选，适合分类、摘要等简单任务。',
    iconPath:
      '<path d="m13 2-2 2.5h3L12 7"/><path d="M12 22v-3"/><circle cx="12" cy="13" r="7"/>',
  },
  {
    vendor: 'OpenAI',
    name: 'GPT-5.5',
    desc: '旗舰多模态模型，胜任写作、推理与视觉理解。',
    iconPath:
      '<path d="M12 2 4 6v6c0 5 3.5 8 8 10 4.5-2 8-5 8-10V6l-8-4z"/><path d="m9 12 2 2 4-4"/>',
  },
  {
    vendor: 'OpenAI',
    name: 'Codex',
    subName: 'GPT-5.3',
    desc: '专为编码场景优化，配合 Codex CLI 使用。',
    iconPath:
      '<polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/>',
  },
];

const features = [
  {
    title: '东京节点低延迟',
    desc: '服务器部署在日本东京，国内访问延迟低、网络稳定，无需自建梯子或翻墙环境。',
    iconPath:
      '<path d="M4 14a1 1 0 0 1-.78-1.63l9.9-10.2a.5.5 0 0 1 .86.46l-1.92 6.02A1 1 0 0 0 13 10h7a1 1 0 0 1 .78 1.63l-9.9 10.2a.5.5 0 0 1-.86-.46l1.92-6.02A1 1 0 0 0 11 14z"/>',
  },
  {
    title: 'OpenAI 协议兼容',
    desc: '完全兼容 OpenAI API 协议，仅需替换 base_url，Codex CLI、Cursor、Cline、Continue.dev 等主流客户端零成本接入。',
    iconPath:
      '<path d="M12 22v-5"/><path d="M9 8V2"/><path d="M15 8V2"/><path d="M18 8v5a4 4 0 0 1-4 4h-4a4 4 0 0 1-4-4V8Z"/>',
  },
  {
    title: 'Token 级实时计费',
    desc: '按 token 实时计费，账单明细可查，无套餐绑定、无最低消费、余额永不过期。',
    iconPath:
      '<path d="M4 2v20l2-1 2 1 2-1 2 1 2-1 2 1 2-1 2 1V2l-2 1-2-1-2 1-2-1-2 1-2-1-2 1Z"/><path d="M8 7h8"/><path d="M8 11h8"/><path d="M8 15h5"/>',
  },
  {
    title: '不降级、不掺水',
    desc: '所有模型保持官方原版调用，不做内容篡改，不私自降级模型，结果与官方 API 一致。',
    iconPath:
      '<path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/><path d="m9 12 2 2 4-4"/>',
  },
  {
    title: '微信支付宝直充',
    desc: '解决 ChatGPT 信用卡限制、虚拟卡风控等支付难题。支持微信、支付宝人民币充值，无需 Apple ID 或海外信用卡。',
    iconPath:
      '<path d="M19 7V5a1 1 0 0 0-1-1H4a1 1 0 0 0-1 1v14a1 1 0 0 0 1 1h15a1 1 0 0 0 1-1v-2"/><path d="M21 12v-2h-5a2 2 0 0 0 0 4h5v-2z"/>',
  },
  {
    title: '余额无忧',
    desc: '充值余额永不过期，可随时申请退款。我们承诺资金不锁定，未消费部分按原路退回。',
    iconPath:
      '<path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.17 1.17 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"/>',
  },
];

const pricing = [
  {
    vendor: 'OpenAI',
    name: 'GPT-5.5 mini',
    desc: '性价比之选，日常对话与高频调用首选。',
    input: '0.5',
    output: '3',
    recommended: false,
  },
  {
    vendor: 'OpenAI',
    name: 'GPT-5.5',
    desc: '当下最强的旗舰大模型，适合复杂对话、长文写作与深度推理。',
    input: '2.5',
    output: '15',
    recommended: true,
  },
  {
    vendor: 'OpenAI',
    name: 'Codex',
    subName: 'GPT-5.3',
    desc: '编码场景专用，配合 Codex CLI 使用。',
    input: '2.5',
    output: '15',
    recommended: false,
  },
];

const wm =
  '<span class="wordmark"><span class="wm-relay">Relay</span><span class="wm-ai">AI</span></span>';

const faqs = [
  {
    q: `${wm} 和直接用官方 API 有什么区别？`,
    a: '我们的服务相当于官方 API 的中转通道——你的请求经我们的服务器转发至上游模型，结果原样返回。优势：1）东京节点直连，无需自建网络环境；2）按真实 token 用量计费，不同模型按上游成本独立定价；3）支持人民币微信、支付宝充值。',
  },
  {
    q: '为什么价格能低于官方？',
    a: '我们通过统一采购与资源池优化降低单位 token 成本，并将节省部分让利给用户。不同模型的折扣幅度会根据上游成本独立设定——目前 GPT 系列约为官方价 50%，后续模型上线时会公布各自的定价。我们承诺不通过"模型掺水""降级到便宜模型"等方式压成本，所有模型保持原版调用。',
  },
  {
    q: '调用质量和官方一致吗？',
    a: '输入 prompt 与输出结果与官方 API 完全一致，不做任何中间篡改或模型降级。通过 <code class="rounded bg-slate-100 px-1 py-0.5 font-mono text-[13px] dark:bg-slate-800">model</code> 字段精确指定调用的模型版本，请求与响应内容透明可验证。',
  },
  {
    q: '服务的稳定性如何？',
    a: '服务器部署在日本东京，配合多上游账号池与故障自动转移保障可用性。作为早期产品，目前不承诺 SLA，但承诺：故障期间不计费、未使用余额可随时退款。',
  },
  {
    q: '如何接入 Codex CLI / Cursor / Cline？',
    a: `详见文档接入指南。简单说就是把客户端的 base_url 替换为 <code class="rounded bg-slate-100 px-1 py-0.5 font-mono text-[13px] dark:bg-slate-800">${apiBaseUrl}</code>，API Key 替换为本站生成的 key，其余配置不变。`,
  },
  {
    q: '支持哪些支付方式？',
    a: '目前支持微信支付、支付宝。后续将开放 USDT、信用卡支付与企业开票。',
  },
  {
    q: '余额能退吗？',
    a: '可以。如决定停止使用，未消费余额按原路退回（扣除支付通道手续费）。我们承诺不锁定用户资金。',
  },
  {
    q: '是否需要担心上游账号封禁？',
    a: `你的 ${wm} 账号与上游模型账号完全隔离。上游账号风险由我们承担，你只需管理好自己的 API Key 即可。`,
  },
];

let observer: IntersectionObserver | null = null;

function onScroll() {
  isScrolled.value = window.scrollY > 4;
}

onMounted(() => {
  initTheme();
  authStore.checkAuth();
  if (!appStore.publicSettingsLoaded) {
    appStore.fetchPublicSettings();
  }

  window.addEventListener('scroll', onScroll, { passive: true });
  onScroll();

  observer = new IntersectionObserver(
    (entries) => {
      for (const entry of entries) {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible');
          observer?.unobserve(entry.target);
        }
      }
    },
    { threshold: 0.1, rootMargin: '0px 0px -40px 0px' },
  );

  requestAnimationFrame(() => {
    document.querySelectorAll('.reveal').forEach((el) => observer?.observe(el));
  });
});

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll);
  observer?.disconnect();
});
</script>

<style scoped>
.relayai-page {
  font-family:
    'Inter',
    'PingFang SC',
    'Microsoft YaHei',
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    sans-serif;
  text-rendering: optimizeLegibility;
  scroll-behavior: smooth;
}

.relayai-page :deep(.font-mono),
.relayai-page :deep(code),
.relayai-page :deep(pre) {
  font-family: 'JetBrains Mono', ui-monospace, SFMono-Regular, Menlo, monospace;
}

.font-cn {
  font-family: 'PingFang SC', 'Microsoft YaHei', 'Inter', system-ui, sans-serif;
}

/* Hero subtle gradient — light + dark */
.hero-bg {
  background: linear-gradient(180deg, #ffffff 0%, #f8fafc 100%);
}
:where(.dark) .hero-bg {
  background: linear-gradient(180deg, #020617 0%, #0f172a 100%);
}

/* Reveal animation */
.reveal {
  opacity: 0;
  transform: translateY(8px);
  transition:
    opacity 400ms ease-out,
    transform 400ms ease-out;
  will-change: opacity, transform;
}
.reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

/* Code block syntax tints (dark code surface) */
.relayai-page :deep(.tok-kw) {
  color: #7dd3fc;
}
.relayai-page :deep(.tok-str) {
  color: #86efac;
}
.relayai-page :deep(.tok-fn) {
  color: #bae6fd;
}
.relayai-page :deep(.tok-var) {
  color: #e2e8f0;
}
.relayai-page :deep(.tok-com) {
  color: #64748b;
}
.relayai-page :deep(.tok-num) {
  color: #fdba74;
}
.relayai-page :deep(.tok-punc) {
  color: #94a3b8;
}

/* Stepper connector */
.step-connector {
  background-image: linear-gradient(to right, #cbd5e1 50%, transparent 50%);
  background-size: 10px 1px;
  background-repeat: repeat-x;
  background-position: center;
  height: 1px;
}
:where(.dark) .step-connector {
  background-image: linear-gradient(to right, #334155 50%, transparent 50%);
}

.step-num {
  display: flex;
  width: 56px;
  height: 56px;
  border-radius: 9999px;
  background: #ffffff;
  border: 1px solid #e2e8f0;
  box-shadow:
    0 1px 2px rgba(15, 23, 42, 0.04),
    0 1px 1px rgba(15, 23, 42, 0.02);
  align-items: center;
  justify-content: center;
  font-family: 'JetBrains Mono', monospace;
  font-size: 18px;
  font-weight: 600;
  color: #155a76;
}
:where(.dark) .step-num {
  background: #0f172a;
  border-color: #1e293b;
  color: #71b3cf;
  box-shadow: none;
}

/* Details / summary chevron */
.relayai-page :deep(details > summary) {
  list-style: none;
  cursor: pointer;
}
.relayai-page :deep(details > summary::-webkit-details-marker) {
  display: none;
}
.relayai-page :deep(details[open] .faq-chevron) {
  transform: rotate(180deg);
}
.relayai-page :deep(.faq-chevron) {
  transition: transform 200ms ease;
}

/* Brand wordmark — :deep so it also styles wordmarks rendered via v-html in FAQ */
.relayai-page :deep(.wordmark) {
  display: inline-flex;
  align-items: baseline;
  letter-spacing: -0.02em;
}
.relayai-page :deep(.wm-relay) {
  font-weight: 600;
  color: #1e293b;
}
.relayai-page :deep(.wm-ai) {
  font-weight: 700;
  color: #155a76;
}
:where(.dark) .relayai-page :deep(.wm-relay) {
  color: #f1f5f9;
}
:where(.dark) .relayai-page :deep(.wm-ai) {
  color: #71b3cf;
}

/* Signal Flow logo mark */
.brand-mark {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
}
.brand-mark .sf-tail,
.brand-mark .sf-main {
  fill: #1e293b;
}
:where(.dark) .brand-mark .sf-tail,
:where(.dark) .brand-mark .sf-main {
  fill: #e2e8f0;
}

/* Primary CTA — primary-500 base in both modes; dark hover goes lighter for visibility */
.brand-cta {
  background-color: #1a6e8e;
}
.brand-cta:hover {
  background-color: #155a76;
}
:where(.dark) .brand-cta {
  background-color: #1a6e8e;
}
:where(.dark) .brand-cta:hover {
  background-color: #3d92b5;
}

/* Ghost CTA */
.brand-ghost {
  color: #155a76;
  border-color: #cbd5e1;
}
.brand-ghost:hover {
  border-color: #71b3cf;
  background-color: #ebf5f9;
}
:where(.dark) .brand-ghost {
  color: #71b3cf;
  border-color: #334155;
}
:where(.dark) .brand-ghost:hover {
  border-color: #3d92b5;
  background-color: rgba(26, 110, 142, 0.12);
}

/* Hero chip */
.brand-chip {
  background: #ebf5f9;
  border: 1px solid #d2e8f1;
  color: #155a76;
}
:where(.dark) .brand-chip {
  background: rgba(26, 110, 142, 0.16);
  border-color: rgba(61, 146, 181, 0.32);
  color: #71b3cf;
}
.brand-chip-dot {
  width: 6px;
  height: 6px;
  border-radius: 9999px;
  background: #1a6e8e;
}
:where(.dark) .brand-chip-dot {
  background: #3d92b5;
}

/* Feature icon tile */
.feat-icon {
  display: inline-flex;
  width: 44px;
  height: 44px;
  border-radius: 12px;
  align-items: center;
  justify-content: center;
  background: #ebf5f9;
  border: 1px solid #d2e8f1;
  color: #155a76;
}
:where(.dark) .feat-icon {
  background: rgba(26, 110, 142, 0.18);
  border-color: rgba(61, 146, 181, 0.32);
  color: #71b3cf;
}

/* Pricing cards — borders via inset box-shadow to avoid layout shift on hover.
   Spotlight pattern: at rest the featured card has the primary ring + glow.
   On hover, ONLY the hovered card has the featured look; if a sibling is being hovered
   the featured card temporarily demotes to the regular ring (no glow). */
.price-card {
  transition:
    transform 200ms ease,
    box-shadow 200ms ease;
  box-shadow: inset 0 0 0 1px #e2e8f0;
}
:where(.dark) .price-card {
  box-shadow: inset 0 0 0 1px #1e293b;
}

.price-card-featured {
  box-shadow:
    inset 0 0 0 2px #1a6e8e,
    0 0 0 4px rgba(21, 94, 117, 0.08);
}
:where(.dark) .price-card-featured {
  box-shadow:
    inset 0 0 0 2px #3d92b5,
    0 0 0 4px rgba(61, 146, 181, 0.18);
}

/* When a non-featured sibling is being hovered, demote the featured card back to plain ring */
.pricing-grid:has(.price-card:hover:not(.price-card-featured))
  .price-card-featured {
  box-shadow: inset 0 0 0 1px #e2e8f0;
}
:where(.dark)
  .pricing-grid:has(.price-card:hover:not(.price-card-featured))
  .price-card-featured {
  box-shadow: inset 0 0 0 1px #1e293b;
}

/* The hovered card — whichever it is — takes on the featured look (no extra deepening) */
.price-card:hover {
  transform: translateY(-2px);
  box-shadow:
    inset 0 0 0 2px #1a6e8e,
    0 0 0 4px rgba(21, 94, 117, 0.08);
}
:where(.dark) .price-card:hover {
  box-shadow:
    inset 0 0 0 2px #3d92b5,
    0 0 0 4px rgba(61, 146, 181, 0.18);
}

/* Pricing tip card */
.pricing-tip {
  background-color: rgba(235, 245, 249, 0.7);
  border-left: 2px solid #155a76;
}
:where(.dark) .pricing-tip {
  background-color: rgba(26, 110, 142, 0.14);
  border-left-color: #3d92b5;
}

/* CTA band */
.cta-band {
  background-color: rgba(235, 245, 249, 0.7);
}
:where(.dark) .cta-band {
  background-color: rgba(26, 110, 142, 0.12);
}

/* Focus ring */
.relayai-page :deep(:focus-visible) {
  outline: 2px solid #155a76;
  outline-offset: 2px;
  border-radius: 6px;
}
:where(.dark) .relayai-page :deep(:focus-visible) {
  outline-color: #71b3cf;
}

/* Card hover */
.lift {
  transition:
    transform 200ms ease,
    box-shadow 200ms ease,
    border-color 200ms ease;
}
.lift:hover {
  transform: translateY(-2px);
  box-shadow:
    0 10px 30px -10px rgba(15, 23, 42, 0.12),
    0 4px 10px -4px rgba(15, 23, 42, 0.06);
  border-color: #71b3cf;
}
:where(.dark) .lift:hover {
  box-shadow:
    0 10px 30px -10px rgba(0, 0, 0, 0.6),
    0 4px 10px -4px rgba(0, 0, 0, 0.45);
  border-color: #3d92b5;
}

/* Nav scrolled shadow */
.nav-scrolled {
  box-shadow:
    0 1px 3px rgba(15, 23, 42, 0.06),
    0 1px 2px rgba(15, 23, 42, 0.04);
}
:where(.dark) .nav-scrolled {
  box-shadow:
    0 1px 3px rgba(0, 0, 0, 0.5),
    0 1px 2px rgba(0, 0, 0, 0.3);
}
</style>
