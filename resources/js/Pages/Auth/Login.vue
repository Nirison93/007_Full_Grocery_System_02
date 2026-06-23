<script setup>
import GuestLayout from "@/Layouts/GuestLayout.vue";
import InputError from "@/Components/InputError.vue";
import { Head, useForm } from "@inertiajs/vue3";
import { ref } from "vue";
import { useI18n } from "vue-i18n";

defineProps({
  canResetPassword: { type: Boolean },
  status: { type: String },
});

const { t, locale } = useI18n();

const form = useForm({
  email: "",
  password: "",
  remember: false,
});

const showPassword = ref(false);
const currentYear = new Date().getFullYear();

const toggleLanguage = () => {
  locale.value = locale.value === 'en' ? 'si' : 'en';
  localStorage.setItem('locale', locale.value);
};

const submit = () => {
  form.post(route("login"), {
    onFinish: () => form.reset("password"),
  });
};
</script>

<template>
  <GuestLayout>
    <Head :title="$t('auth.sign_in')" />

    <div class="login-root">

      <!-- ── Left Panel ── -->
      <div class="left-panel">
        <div class="grid-bg" />
        <div class="orb orb-1" />
        <div class="orb orb-2" />

        <div class="left-content">
          <div class="badge">
            <span class="badge-dot" />
            <span class="badge-text">POS System</span>
          </div>

          <h1 class="hero-title">
            {{ $t('auth.hero_title_1') }}<br />
            {{ $t('auth.hero_title_2') }} <em>{{ $t('auth.hero_title_em') }}</em>
          </h1>

          <p class="hero-sub">{{ $t('auth.hero_sub') }}</p>

          <div class="stats-row">
            <div class="stat">
              <span class="stat-num">12k+</span>
              <span class="stat-lbl">{{ $t('auth.active_users') }}</span>
            </div>
            <div class="stat-divider" />
            <div class="stat">
              <span class="stat-num">99.9%</span>
              <span class="stat-lbl">{{ $t('auth.uptime') }}</span>
            </div>
            <div class="stat-divider" />
            <div class="stat">
              <span class="stat-num">LKR</span>
              <span class="stat-lbl">{{ $t('auth.local_support') }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- ── Right Panel – Form ── -->
      <div class="right-panel">
        <div class="form-card">

          <!-- Logo -->
          <div class="logo-row">
            <div class="logo-icon">
              <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M13 10V3L4 14h7v7l9-11h-7z" />
              </svg>
            </div>
            <div>
              <p class="logo-name">Online මුදලාලි</p>
              <p class="logo-sub">{{ $t('auth.point_of_sale') }}</p>
            </div>
            <!-- Language toggle on login page -->
            <button
              @click="toggleLanguage"
              type="button"
              class="ml-auto inline-flex items-center gap-1 px-3 py-1.5 rounded-md border border-gray-300 text-xs font-bold tracking-wide transition-all hover:bg-gray-100 select-none"
            >
              <span :class="locale === 'en' ? 'text-gray-900' : 'text-gray-400'">EN</span>
              <span class="text-gray-400">|</span>
              <span :class="locale === 'si' ? 'text-blue-600' : 'text-gray-400'">සිං</span>
            </button>
          </div>

          <!-- Heading -->
          <h2 class="form-title">{{ $t('auth.welcome_back') }}</h2>
          <p class="form-sub">{{ $t('auth.sign_in_subtitle') }}</p>

          <!-- Status -->
          <div v-if="status" class="status-msg">
            <svg class="status-icon" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                clip-rule="evenodd" />
            </svg>
            <span>{{ status }}</span>
          </div>

          <!-- Form -->
          <form @submit.prevent="submit" class="login-form">

            <!-- Email -->
            <div class="field">
              <label for="email" class="field-label">{{ $t('auth.email_address') }}</label>
              <div class="field-wrap">
                <svg class="field-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                    d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                </svg>
                <input
                  id="email"
                  v-model="form.email"
                  type="email"
                  autocomplete="username"
                  :placeholder="$t('auth.email_placeholder')"
                  required
                  autofocus
                  class="field-input"
                  :class="{ 'field-input--error': form.errors.email }"
                />
              </div>
              <InputError :message="form.errors.email" class="field-error" />
            </div>

            <!-- Password -->
            <div class="field">
              <div class="field-header">
                <label for="password" class="field-label">{{ $t('auth.password') }}</label>
                <a v-if="canResetPassword" :href="route('password.request')" class="forgot-link">
                  {{ $t('auth.forgot_password') }}
                </a>
              </div>
              <div class="field-wrap">
                <svg class="field-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                    d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                </svg>
                <input
                  id="password"
                  v-model="form.password"
                  :type="showPassword ? 'text' : 'password'"
                  autocomplete="current-password"
                  :placeholder="$t('auth.password_placeholder')"
                  required
                  class="field-input field-input--padded"
                  :class="{ 'field-input--error': form.errors.password }"
                />
                <button
                  type="button"
                  @click="showPassword = !showPassword"
                  class="eye-btn"
                  :aria-label="showPassword ? $t('auth.hide_password') : $t('auth.show_password')"
                >
                  <svg v-if="!showPassword" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                      d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                      d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
                  </svg>
                  <svg v-else fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                      d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29m7.532 7.532l3.29 3.29M3 3l3.59 3.59m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943 9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21" />
                  </svg>
                </button>
              </div>
              <InputError :message="form.errors.password" class="field-error" />
            </div>

            <!-- Submit -->
            <button
              type="submit"
              class="submit-btn"
              :class="{ 'submit-btn--loading': form.processing }"
              :disabled="form.processing"
            >
              <span v-if="!form.processing" class="submit-inner">
                {{ $t('auth.sign_in') }}
                <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M17 8l4 4m0 0l-4 4m4-4H3" />
                </svg>
              </span>
              <span v-else class="submit-inner">
                <svg class="spin" fill="none" viewBox="0 0 24 24">
                  <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4" />
                  <path class="opacity-75" fill="currentColor"
                    d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z" />
                </svg>
                {{ $t('auth.signing_in') }}
              </span>
            </button>

          </form>

          <p class="copyright">© {{ currentYear }} Online මුදලාලි. {{ $t('auth.copyright').replace('© {year} Online මුදලාලි. ', '') }}</p>
        </div>
      </div>

    </div>
  </GuestLayout>
</template>

<style scoped>
.login-root {
  display: flex;
  height: 100vh;
  overflow: hidden;
  background: #0a0a0f;
  font-family: 'Figtree', 'Inter', system-ui, sans-serif;
}

.left-panel {
  display: none;
  position: relative;
  overflow: hidden;
  background: #0a0a0f;
}
@media (min-width: 1024px) {
  .left-panel { display: flex; flex: 1; align-items: flex-end; padding: 56px; }
}

.grid-bg {
  position: absolute; inset: 0;
  background-image:
    linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
  background-size: 40px 40px;
}

.orb { position: absolute; border-radius: 50%; pointer-events: none; }
.orb-1 {
  top: -120px; left: -120px;
  width: 560px; height: 560px;
  background: radial-gradient(circle, rgba(99,102,241,0.22) 0%, transparent 65%);
}
.orb-2 {
  bottom: 60px; right: -100px;
  width: 400px; height: 400px;
  background: radial-gradient(circle, rgba(168,85,247,0.14) 0%, transparent 65%);
}

.left-content { position: relative; z-index: 2; }

.badge {
  display: inline-flex; align-items: center; gap: 8px;
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 100px;
  padding: 7px 16px;
  margin-bottom: 28px;
}
.badge-dot { width: 7px; height: 7px; border-radius: 50%; background: #6ee7b7; box-shadow: 0 0 6px #6ee7b7; }
.badge-text { font-size: 11px; font-weight: 500; color: rgba(255,255,255,0.55); letter-spacing: 0.1em; text-transform: uppercase; }

.hero-title {
  font-size: clamp(36px, 4vw, 56px);
  font-weight: 700; line-height: 1.1;
  color: #fff; letter-spacing: -0.03em; margin-bottom: 20px;
}
.hero-title em {
  font-style: normal;
  background: linear-gradient(135deg, #a78bfa 0%, #6366f1 100%);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;
}

.hero-sub { font-size: 15px; color: rgba(255,255,255,0.42); line-height: 1.75; max-width: 360px; margin-bottom: 48px; }

.stats-row { display: flex; align-items: center; gap: 28px; }
.stat { display: flex; flex-direction: column; gap: 4px; }
.stat-num { font-size: 20px; font-weight: 700; color: #fff; letter-spacing: -0.02em; }
.stat-lbl { font-size: 11px; color: rgba(255,255,255,0.32); text-transform: uppercase; letter-spacing: 0.06em; }
.stat-divider { width: 1px; height: 36px; background: rgba(255,255,255,0.08); }

.right-panel {
  width: 100%; background: #fff;
  display: flex; align-items: center; justify-content: center;
  padding: 32px 24px; overflow-y: auto;
}
@media (min-width: 1024px) { .right-panel { width: 480px; flex-shrink: 0; } }

.form-card { width: 100%; max-width: 380px; }

.logo-row { display: flex; align-items: center; gap: 12px; margin-bottom: 40px; }
.logo-icon {
  width: 38px; height: 38px; background: #0a0a0f; border-radius: 10px;
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
}
.logo-icon svg { width: 18px; height: 18px; stroke: #fff; }
.logo-name { font-size: 15px; font-weight: 700; color: #0a0a0f; letter-spacing: -0.01em; line-height: 1.2; }
.logo-sub { font-size: 11px; color: #9ca3af; font-weight: 400; margin-top: 1px; }

.form-title { font-size: 28px; font-weight: 700; color: #0a0a0f; letter-spacing: -0.025em; margin-bottom: 6px; }
.form-sub { font-size: 14px; color: #9ca3af; margin-bottom: 32px; }

.status-msg {
  display: flex; align-items: center; gap: 10px;
  background: #f0fdf4; border: 1px solid #bbf7d0; border-radius: 10px;
  padding: 12px 16px; margin-bottom: 24px; font-size: 13px; font-weight: 500; color: #166534;
}
.status-icon { width: 16px; height: 16px; flex-shrink: 0; }

.login-form { display: flex; flex-direction: column; gap: 20px; }
.field { display: flex; flex-direction: column; gap: 8px; }
.field-header { display: flex; justify-content: space-between; align-items: center; }
.field-label { font-size: 11px; font-weight: 600; color: #374151; letter-spacing: 0.07em; text-transform: uppercase; }
.forgot-link { font-size: 12px; color: #6366f1; text-decoration: none; font-weight: 500; transition: color 0.15s; }
.forgot-link:hover { color: #4338ca; }
.field-wrap { position: relative; }
.field-icon { position: absolute; left: 14px; top: 50%; transform: translateY(-50%); width: 16px; height: 16px; stroke: #d1d5db; pointer-events: none; }
.field-input {
  width: 100%; height: 46px; border: 1.5px solid #e5e7eb; border-radius: 10px;
  padding: 0 14px 0 40px; font-size: 14px; color: #0a0a0f; background: #fafafa;
  font-family: inherit; outline: none; transition: border-color 0.15s, background 0.15s, box-shadow 0.15s;
}
.field-input::placeholder { color: #d1d5db; }
.field-input:focus { border-color: #6366f1; background: #fff; box-shadow: 0 0 0 3px rgba(99,102,241,0.08); }
.field-input--padded { padding-right: 44px; }
.field-input--error { border-color: #f87171; }
.field-input--error:focus { box-shadow: 0 0 0 3px rgba(248,113,113,0.1); }
.eye-btn {
  position: absolute; right: 12px; top: 50%; transform: translateY(-50%);
  background: none; border: none; cursor: pointer; padding: 4px; color: #9ca3af;
  display: flex; align-items: center; justify-content: center; border-radius: 6px; transition: color 0.15s;
}
.eye-btn:hover { color: #6b7280; }
.eye-btn svg { width: 17px; height: 17px; }
.field-error { font-size: 12px; color: #ef4444; margin-top: -4px; }

.submit-btn {
  width: 100%; height: 48px; background: #0a0a0f; border: none; border-radius: 10px;
  color: #fff; font-size: 14px; font-weight: 600; font-family: inherit; cursor: pointer;
  letter-spacing: 0.01em; transition: background 0.2s, transform 0.1s, box-shadow 0.2s;
  margin-top: 4px; box-shadow: 0 2px 8px rgba(10,10,15,0.18);
}
.submit-btn:hover:not(:disabled) { background: #1e1b4b; box-shadow: 0 4px 16px rgba(10,10,15,0.28); transform: translateY(-1px); }
.submit-btn:active:not(:disabled) { transform: translateY(0); }
.submit-btn--loading, .submit-btn:disabled { opacity: 0.6; cursor: not-allowed; }
.submit-inner { display: flex; align-items: center; justify-content: center; gap: 8px; }
.submit-inner svg { width: 16px; height: 16px; }
.spin { animation: spin 0.9s linear infinite; }
@keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }

.copyright { text-align: center; font-size: 12px; color: #d1d5db; margin-top: 32px; }
</style>
