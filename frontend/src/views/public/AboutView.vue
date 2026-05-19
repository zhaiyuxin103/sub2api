<template>
  <div class="relayai-about min-h-screen bg-white text-slate-900 antialiased dark:bg-slate-950 dark:text-slate-100">
    <!-- Header -->
    <header class="border-b border-slate-200 bg-white/95 dark:border-slate-800 dark:bg-slate-950/95">
      <div class="mx-auto flex max-w-5xl items-center justify-between gap-4 px-6 py-4">
        <router-link to="/" class="flex items-center gap-2.5">
          <span class="brand-mark">
            <img src="/mark.svg" alt="" class="brand-mark-img dark:hidden" />
            <img src="/mark-dark.svg" alt="" class="brand-mark-img hidden dark:block" />
          </span>
          <span class="wordmark text-[18px]"><span class="wm-relay">Relay</span><span class="wm-ai">AI</span></span>
        </router-link>
        <router-link
          to="/"
          class="text-[14px] text-slate-500 transition-colors hover:text-slate-900 dark:text-slate-400 dark:hover:text-white"
          >返回首页</router-link
        >
      </div>
    </header>

    <main class="mx-auto max-w-3xl px-6 py-14 md:py-20">
      <p class="eyebrow font-mono text-[12px] uppercase tracking-[0.18em] text-slate-500 dark:text-slate-400">
        About · Relay AI
      </p>
      <h1 class="font-cn mt-3 text-[32px] font-bold leading-[1.2] tracking-tight text-slate-900 dark:text-white md:text-[44px]">
        我们想做一个<br />值得被开发者信任的 AI 中转
      </h1>

      <div class="prose-cn mt-10 space-y-12">
        <section>
          <h2 class="section-h">我们是谁</h2>
          <p>
            Relay AI 是一家独立运营的 AI 模型 API 服务，于 2026 年初上线，致力于为开发者提供稳定、低延迟、价格透明的 AI 大语言模型接入能力。
          </p>
          <p>
            我们不是一家公司 —— 至少现在还不是。Relay AI 由独立开发者运营，没有融资，没有团队扩张计划。所有产品决策由运营方一人完成，所有承诺直接执行。这是它「小」的一面，也是它「快」的一面。
          </p>
        </section>

        <section>
          <h2 class="section-h">我们想解决什么</h2>
          <p>接入海外 AI 模型的真实痛点对国内开发者很熟悉：</p>
          <ul class="point-list">
            <li><strong>支付难</strong>：国内信用卡 / 借记卡不被接受，Apple ID 与虚拟卡风控严</li>
            <li><strong>访问慢 / 不稳</strong>：境外服务直连延迟高，自建网络环境维护成本大</li>
            <li><strong>配额受限</strong>：个人账号 RPM / TPM 上限低，生产场景容易触发限流</li>
            <li><strong>多账号管理</strong>：同时接入 OpenAI、Claude、Gemini 要分别开户、分别充值、分别监控</li>
          </ul>
          <p>Relay AI 把这些打包解决 —— 一个 API Key 调通所有主流模型，人民币充值，按 token 计费。</p>
        </section>

        <section>
          <h2 class="section-h">我们的承诺</h2>
          <p>作为一家「小而早」的服务，信任只能靠诚实和兑现累积。所以这几件事我们想说在前面：</p>
          <ol class="point-list numbered">
            <li>
              <strong>100% 调用官方原版模型</strong> —— 不做后台智能路由、不替换、不降级。<strong class="text-primary-600 dark:text-primary-300">假一赔十</strong>。
            </li>
            <li>
              <strong>不持久化你的调用内容</strong> —— 默认情况下，你发给模型的 prompt 与模型返回的 completion 在转发后不做存储。仅在你主动报障 / 触发滥用检测时临时记录，排查完毕清除。
            </li>
            <li>
              <strong>资金不锁定</strong> —— 余额永不过期，未消费部分可随时按原路申请退款。
            </li>
            <li>
              <strong>价格透明</strong> —— 定价以官网展示为准，上游成本变动时同步调整，调整后通过公告告知。不会在你不知情的情况下偷涨。
            </li>
          </ol>
        </section>

        <section>
          <h2 class="section-h">服务架构</h2>
          <ul class="point-list">
            <li><strong>节点</strong>：日本东京，境外服务器，国内访问稳定低延迟</li>
            <li><strong>已上线模型</strong>：OpenAI 全系（GPT-5.5、GPT-5.5 mini、Codex)</li>
            <li><strong>规划中</strong>：Anthropic Claude(Opus / Sonnet）、Google Gemini</li>
            <li><strong>协议兼容</strong>：完整兼容 OpenAI API,Codex CLI / Cursor / Cline / Chatbox / NextChat / OpenCat 等主流客户端零成本接入</li>
          </ul>
        </section>

        <section>
          <h2 class="section-h">联系我们</h2>
          <ul class="point-list contacts">
            <li><span class="label">客服邮箱</span><a class="link" href="mailto:support@relayai.asia">support@relayai.asia</a></li>
            <li><span class="label">QQ 交流群</span><span class="font-mono">263891281</span></li>
            <li>
              <span class="label">微信交流群</span>
              <button type="button" class="link link-button" @click="wechatModalOpen = true">扫码加入</button>
            </li>
            <li><span class="label">官网</span><a class="link" href="https://relayai.asia" target="_blank" rel="noopener">https://relayai.asia</a></li>
          </ul>
        </section>

        <section class="closing">
          <p>
            如果你正在使用 Relay AI，无论是付费用户还是体验用户，你的每一次反馈都直接影响这项服务的下一步演化。
            有问题、有想法、有抱怨，都欢迎到 QQ 群、微信群或邮件告诉我们。
          </p>
        </section>
      </div>

      <div class="mt-16 border-t border-slate-200 pt-8 dark:border-slate-800">
        <router-link
          to="/"
          class="inline-flex items-center gap-2 text-[14px] font-medium text-primary-600 transition-colors hover:text-primary-700 dark:text-primary-300 dark:hover:text-primary-200"
        >
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M19 12H5" />
            <path d="m12 19-7-7 7-7" />
          </svg>
          返回首页
        </router-link>
      </div>
    </main>

    <!-- WeChat Group Modal -->
    <Teleport to="body">
      <Transition name="wechat-fade">
        <div
          v-if="wechatModalOpen"
          class="wechat-overlay fixed inset-0 z-[100] flex items-center justify-center p-4"
          @click.self="wechatModalOpen = false"
        >
          <div
            class="wechat-dialog relative w-full max-w-sm rounded-2xl bg-white p-6 shadow-2xl dark:bg-slate-900"
          >
            <button
              type="button"
              class="absolute right-3 top-3 rounded-lg p-1.5 text-slate-400 transition-colors hover:bg-slate-100 hover:text-slate-700 dark:text-slate-500 dark:hover:bg-slate-800 dark:hover:text-slate-200"
              aria-label="关闭"
              @click="wechatModalOpen = false"
            >
              <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <path d="M18 6 6 18" />
                <path d="m6 6 12 12" />
              </svg>
            </button>

            <div class="text-center">
              <p class="eyebrow font-mono text-[11px] uppercase tracking-[0.18em] text-slate-500 dark:text-slate-400">
                WeChat Group
              </p>
              <h3 class="font-cn mt-2 text-[18px] font-semibold text-slate-900 dark:text-white">
                Relay AI 交流群
              </h3>
              <p class="font-cn mt-1 text-[13px] text-slate-500 dark:text-slate-400">
                客服支持、使用反馈、产品建议均可在群内交流
              </p>
            </div>

            <div class="mt-5 rounded-xl bg-slate-50 p-4 dark:bg-slate-800/60">
              <img
                src="/wechat-qr.png"
                alt="微信群二维码"
                class="mx-auto block w-full max-w-[220px] rounded-lg"
                loading="lazy"
              />
            </div>

            <p class="font-cn mt-4 text-center text-[12px] text-slate-400 dark:text-slate-500">
              微信扫码加入；如二维码失效请邮件联系
              <a class="ml-1 font-mono text-slate-500 hover:text-slate-700 dark:text-slate-400 dark:hover:text-slate-200" href="mailto:support@relayai.asia">support@relayai.asia</a>
            </p>
          </div>
        </div>
      </Transition>
    </Teleport>
  </div>
</template>

<script setup lang="ts">
// Static About page — content edited directly in template until we have a CMS
import { onMounted, ref } from 'vue'

const wechatModalOpen = ref(false)

onMounted(() => {
  document.title = '关于 Relay AI · AI 模型 API 高速通道'
})
</script>

<style scoped>
.relayai-about {
  font-family: 'Inter', 'PingFang SC', 'Microsoft YaHei', system-ui, -apple-system, sans-serif;
}
.font-cn {
  font-family: 'PingFang SC', 'Microsoft YaHei', 'Inter', system-ui, sans-serif;
}

.brand-mark {
  display: inline-flex;
  align-items: center;
  line-height: 0;
}
.brand-mark-img {
  height: 24px;
  width: auto;
}

.wordmark {
  display: inline-flex;
  align-items: baseline;
  letter-spacing: -0.035em;
  line-height: 0.95;
}
.wm-relay {
  font-weight: 800;
  color: #1e293b;
}
.wm-ai {
  font-weight: 900;
  color: #0d9488;
}
:where(.dark) .wm-relay {
  color: #f1f5f9;
}
:where(.dark) .wm-ai {
  color: #14b8a6;
}

.eyebrow {
  font-feature-settings: 'tnum';
}

.section-h {
  font-family: 'PingFang SC', 'Microsoft YaHei', 'Inter', system-ui, sans-serif;
  font-size: 20px;
  font-weight: 700;
  letter-spacing: -0.01em;
  color: #0f172a;
  margin-bottom: 16px;
}
:where(.dark) .section-h {
  color: #f8fafc;
}

.prose-cn {
  font-family: 'PingFang SC', 'Microsoft YaHei', 'Inter', system-ui, sans-serif;
}
.prose-cn p {
  font-size: 15.5px;
  line-height: 1.85;
  color: #475569;
}
:where(.dark) .prose-cn p {
  color: #cbd5e1;
}
.prose-cn p + p {
  margin-top: 12px;
}

.point-list {
  margin-top: 12px;
  padding-left: 0;
  list-style: none;
}
.point-list li {
  position: relative;
  padding-left: 22px;
  font-size: 15px;
  line-height: 1.85;
  color: #475569;
}
:where(.dark) .point-list li {
  color: #cbd5e1;
}
.point-list li + li {
  margin-top: 6px;
}
.point-list li::before {
  content: '';
  position: absolute;
  left: 6px;
  top: 0.8em;
  width: 6px;
  height: 6px;
  border-radius: 9999px;
  background: #1a6e8e;
}
:where(.dark) .point-list li::before {
  background: #3d92b5;
}
.point-list.numbered {
  counter-reset: section;
}
.point-list.numbered li {
  padding-left: 32px;
  counter-increment: section;
}
.point-list.numbered li::before {
  content: counter(section);
  position: absolute;
  left: 0;
  top: 0;
  width: 22px;
  height: 22px;
  border-radius: 9999px;
  background: #ebf5f9;
  color: #155a76;
  font-family: 'JetBrains Mono', ui-monospace, monospace;
  font-size: 12px;
  font-weight: 600;
  line-height: 22px;
  text-align: center;
}
:where(.dark) .point-list.numbered li::before {
  background: rgba(26, 110, 142, 0.18);
  color: #71b3cf;
}
.point-list.contacts li {
  display: flex;
  gap: 12px;
  padding-left: 0;
}
.point-list.contacts li::before {
  display: none;
}
.point-list.contacts .label {
  flex-shrink: 0;
  width: 80px;
  font-size: 14px;
  color: #64748b;
}
:where(.dark) .point-list.contacts .label {
  color: #94a3b8;
}
.point-list.contacts .link {
  color: #155a76;
  font-family: 'JetBrains Mono', ui-monospace, monospace;
  font-size: 14px;
  text-decoration: none;
}
.point-list.contacts .link:hover {
  text-decoration: underline;
}
:where(.dark) .point-list.contacts .link {
  color: #71b3cf;
}
.point-list.contacts .link-button {
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
}

.closing p {
  border-left: 2px solid #155a76;
  padding-left: 16px;
  font-style: italic;
  font-size: 15px;
  color: #475569;
}
:where(.dark) .closing p {
  border-left-color: #3d92b5;
  color: #94a3b8;
}

/* WeChat group modal */
.wechat-overlay {
  background: rgba(15, 23, 42, 0.55);
  backdrop-filter: blur(4px);
}
:where(.dark) .wechat-overlay {
  background: rgba(0, 0, 0, 0.65);
}
.wechat-dialog {
  box-shadow:
    0 20px 60px -20px rgba(15, 23, 42, 0.25),
    0 8px 20px -8px rgba(15, 23, 42, 0.12);
}
:where(.dark) .wechat-dialog {
  box-shadow:
    0 20px 60px -20px rgba(0, 0, 0, 0.7),
    0 8px 20px -8px rgba(0, 0, 0, 0.5);
}
.wechat-fade-enter-active,
.wechat-fade-leave-active {
  transition:
    opacity 200ms ease,
    transform 200ms ease;
}
.wechat-fade-enter-active .wechat-dialog,
.wechat-fade-leave-active .wechat-dialog {
  transition:
    opacity 200ms ease,
    transform 200ms ease;
}
.wechat-fade-enter-from,
.wechat-fade-leave-to {
  opacity: 0;
}
.wechat-fade-enter-from .wechat-dialog,
.wechat-fade-leave-to .wechat-dialog {
  opacity: 0;
  transform: translateY(8px) scale(0.98);
}
</style>
