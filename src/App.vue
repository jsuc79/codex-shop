<script setup>
import { computed, reactive, ref } from "vue";

const dishes = [
  {
    name: "泰式柠檬鲈鱼",
    description: "鲜嫩鲈鱼、青柠酸汤、香茅辣椒、清爽开胃",
    price: "¥48",
    image: "https://images.unsplash.com/photo-1546069901-ba9599a7e63c?auto=format&fit=crop&w=900&q=80",
    alt: "酸辣清香的泰式柠檬鲈鱼",
  },
  {
    name: "滑蛋多汁嫩虾仁",
    description: "现剥虾仁、嫩滑鸡蛋、葱香热油、鲜甜多汁",
    price: "¥138",
    image: "https://images.unsplash.com/photo-1559847844-5315695dadae?auto=format&fit=crop&w=900&q=80",
    alt: "金黄滑蛋包裹鲜嫩虾仁",
    featured: true,
  },
  {
    name: "澳洲新鲜生蚝",
    description: "冷链鲜运、海盐矿物感、柠檬点缀、入口清甜",
    price: "¥68",
    image: "https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?auto=format&fit=crop&w=900&q=80",
    alt: "冰镇摆盘的澳洲新鲜生蚝",
  },
];

const partyOptions = ["2位", "3-4位", "5-6位", "包间"];
const timeOptions = ["18:00", "18:30", "19:00", "19:30", "20:00"];

const wishes = ref([]);
const isDrawerOpen = ref(false);
const addedDish = ref("");
const formMessage = ref("");
const reservation = reactive({
  name: "",
  party: partyOptions[0],
  date: "",
  time: timeOptions[0],
});

const today = computed(() => new Date().toISOString().split("T")[0]);

function addWish(dishName) {
  if (!wishes.value.includes(dishName)) {
    wishes.value.push(dishName);
  }

  addedDish.value = dishName;
  isDrawerOpen.value = true;

  window.setTimeout(() => {
    if (addedDish.value === dishName) {
      addedDish.value = "";
    }
  }, 1500);
}

function closeDrawer() {
  isDrawerOpen.value = false;
}

function submitReservation() {
  formMessage.value = `${reservation.name}，已收到您 ${reservation.date} ${reservation.time} 的${reservation.party}订位请求。`;
  reservation.name = "";
  reservation.party = partyOptions[0];
  reservation.date = "";
  reservation.time = timeOptions[0];
}
</script>

<template>
  <header class="site-header" id="top">
    <a class="brand" href="#top" aria-label="青禾小馆首页">
      <span class="brand-mark">青</span>
      <span>青禾小馆</span>
    </a>

    <nav class="main-nav" aria-label="主导航">
      <a href="#menu">菜单</a>
      <a href="#story">空间</a>
      <a href="#visit">订位</a>
      <a href="#hours">营业</a>
    </nav>

    <div class="header-actions">
      <a class="ghost-link" href="tel:+8613800000000">致电</a>
      <a class="button header-button" href="#visit">订座</a>
    </div>
  </header>

  <main>
    <section class="hero" aria-labelledby="hero-title">
      <div class="hero-copy">
        <p class="eyebrow">当季食材 · 明火慢烹 · 城市晚餐</p>
        <h1 id="hero-title">青禾小馆</h1>
        <p class="hero-text">
          一间安静但有烟火气的现代中式餐厅。白天供应轻盈午餐，夜晚以炭火、时令蔬食和手作点心迎接朋友相聚。
        </p>
        <div class="hero-actions">
          <a class="button primary" href="#visit">预约今晚座位</a>
          <a class="button secondary" href="#menu">查看招牌菜单</a>
        </div>
      </div>

      <div class="hero-note" aria-label="餐厅亮点">
        <span>人均 ¥168</span>
        <span>今日 11:30 - 22:00</span>
        <span>可包间 · 可外带</span>
      </div>
    </section>

    <section class="section-wrap intro" aria-label="餐厅简介">
      <div>
        <p class="eyebrow">Today at Qinghe</p>
        <h2>慢火做菜，也认真照顾每一次落座。</h2>
      </div>
      <p>
        厨房每天从附近市场挑选蔬菜、河鲜与香草，菜单随季节小幅变化。前厅保留舒适的桌距、柔和灯光和开放式吧台，适合两人晚餐、朋友聚会和轻商务餐。
      </p>
    </section>

    <section class="menu section-wrap" id="menu" aria-labelledby="menu-title">
      <div class="section-heading split">
        <div>
          <p class="eyebrow">Signature Menu</p>
          <h2 id="menu-title">推荐菜品</h2>
        </div>
        <a class="text-link" href="#visit">想吃哪道，订位时备注</a>
      </div>

      <div class="dish-grid">
        <article
          v-for="dish in dishes"
          :key="dish.name"
          class="dish-card"
          :class="{ 'featured-dish': dish.featured }"
        >
          <img :src="dish.image" :alt="dish.alt" />
          <div class="dish-info">
            <div>
              <h3>{{ dish.name }}</h3>
              <p>{{ dish.description }}</p>
            </div>
            <strong>{{ dish.price }}</strong>
          </div>
          <button
            class="button order-button"
            :class="{ added: addedDish === dish.name }"
            type="button"
            @click="addWish(dish.name)"
          >
            {{ addedDish === dish.name ? "已加入清单" : "加入想吃清单" }}
          </button>
        </article>
      </div>
    </section>

    <section class="story" id="story" aria-labelledby="story-title">
      <img
        src="https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?auto=format&fit=crop&w=1400&q=80"
        alt="温暖灯光下的餐厅室内空间"
      />
      <div class="story-copy">
        <p class="eyebrow">Invite Friends</p>
        <h2 id="story-title">推荐好友来吃饭，双方都有奖励。</h2>
        <p>
          把青禾小馆分享给朋友，好友首次到店用餐后，你将获得下次消费抵扣券。好友也能领取新人礼，一起把好吃的晚餐安排上。
        </p>
        <div class="story-list" aria-label="餐厅服务">
          <span>推荐得奖励</span>
          <span>好友享新人礼</span>
          <span>下次用餐可抵扣</span>
        </div>
      </div>
    </section>

    <section class="visit section-wrap" id="visit" aria-labelledby="visit-title">
      <div class="reservation-panel">
        <div>
          <p class="eyebrow">Reserve</p>
          <h2 id="visit-title">预留一张好好吃饭的桌子</h2>
        </div>
        <form class="reservation-form" aria-label="订位表单" @submit.prevent="submitReservation">
          <label>
            姓名
            <input v-model="reservation.name" name="name" type="text" placeholder="怎么称呼您" required />
          </label>
          <label>
            人数
            <select v-model="reservation.party" name="party" required>
              <option v-for="party in partyOptions" :key="party" :value="party">{{ party }}</option>
            </select>
          </label>
          <label>
            日期
            <input v-model="reservation.date" name="date" type="date" :min="today" required />
          </label>
          <label>
            时间
            <select v-model="reservation.time" name="time" required>
              <option v-for="time in timeOptions" :key="time" :value="time">{{ time }}</option>
            </select>
          </label>
          <button class="button primary" type="submit">提交订位</button>
          <p class="form-message" role="status" aria-live="polite">{{ formMessage }}</p>
        </form>
      </div>
    </section>

    <section class="hours" id="hours" aria-labelledby="hours-title">
      <div>
        <p class="eyebrow">Visit Us</p>
        <h2 id="hours-title">营业信息</h2>
      </div>
      <div class="hours-grid">
        <div>
          <span>地址</span>
          <strong>上海市静安区梧桐路 26 号</strong>
        </div>
        <div>
          <span>午餐</span>
          <strong>11:30 - 14:30</strong>
        </div>
        <div>
          <span>晚餐</span>
          <strong>17:30 - 22:00</strong>
        </div>
        <div>
          <span>电话</span>
          <strong>+86 138 0000 0000</strong>
        </div>
      </div>
    </section>
  </main>

  <aside class="wish-drawer" :class="{ open: isDrawerOpen }" aria-label="想吃清单" :aria-hidden="!isDrawerOpen">
    <div class="drawer-header">
      <strong>想吃清单</strong>
      <button class="icon-button drawer-close" type="button" aria-label="关闭想吃清单" @click="closeDrawer">
        ×
      </button>
    </div>
    <div class="wish-items" data-wish-items>
      <p v-if="wishes.length === 0">还没有添加菜品。</p>
      <div v-for="item in wishes" v-else :key="item" class="wish-item">
        <span>{{ item }}</span>
        <strong>已记录</strong>
      </div>
    </div>
  </aside>

  <footer class="site-footer">
    <span>青禾小馆</span>
    <a href="#top">回到顶部</a>
  </footer>
</template>

<style>
:root {
  --ink: #171512;
  --muted: #6e665d;
  --paper: #faf7ef;
  --linen: #efe7d8;
  --leaf: #2f5d46;
  --leaf-deep: #183f31;
  --tomato: #b94b34;
  --saffron: #d89a35;
  --white: #fffdf8;
  --line: rgba(23, 21, 18, 0.14);
  --shadow: 0 22px 70px rgba(37, 30, 20, 0.16);
  color-scheme: light;
}

* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  background: var(--paper);
  color: var(--ink);
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

img {
  display: block;
  max-width: 100%;
}

a {
  color: inherit;
  text-decoration: none;
}

button,
input,
select {
  font: inherit;
}

.site-header {
  position: sticky;
  top: 0;
  z-index: 30;
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 26px;
  padding: 14px clamp(18px, 4vw, 58px);
  border-bottom: 1px solid rgba(255, 253, 248, 0.18);
  background: rgba(23, 21, 18, 0.78);
  color: var(--white);
  backdrop-filter: blur(18px);
}

.brand,
.main-nav,
.header-actions,
.hero-actions,
.hero-note,
.dish-info,
.story-list,
.drawer-header,
.site-footer {
  display: flex;
  align-items: center;
}

.brand {
  gap: 10px;
  font-size: 1.1rem;
  font-weight: 850;
  letter-spacing: 0;
}

.brand-mark {
  display: grid;
  width: 36px;
  height: 36px;
  place-items: center;
  border: 1px solid rgba(255, 253, 248, 0.54);
  border-radius: 50%;
  color: #f4d58a;
}

.main-nav {
  justify-content: center;
  gap: clamp(14px, 3vw, 34px);
  color: rgba(255, 253, 248, 0.76);
  font-size: 0.94rem;
}

.main-nav a:hover,
.ghost-link:hover,
.site-footer a:hover,
.text-link:hover {
  color: var(--saffron);
}

.header-actions {
  justify-content: flex-end;
  gap: 12px;
}

.ghost-link {
  color: rgba(255, 253, 248, 0.78);
  font-weight: 750;
}

.button {
  display: inline-flex;
  min-height: 46px;
  align-items: center;
  justify-content: center;
  border: 1px solid var(--leaf);
  border-radius: 999px;
  padding: 0 20px;
  background: transparent;
  color: var(--ink);
  cursor: pointer;
  font-weight: 800;
  transition: transform 180ms ease, background 180ms ease, border-color 180ms ease;
}

.button:hover {
  transform: translateY(-1px);
}

.button.primary,
.header-button {
  border-color: var(--leaf);
  background: var(--leaf);
  color: var(--white);
}

.header-button {
  min-height: 40px;
  padding-inline: 18px;
}

.button.secondary {
  border-color: rgba(255, 253, 248, 0.7);
  background: rgba(255, 253, 248, 0.14);
  color: var(--white);
}

.hero {
  position: relative;
  display: grid;
  min-height: calc(100vh - 66px);
  align-items: end;
  overflow: hidden;
  padding: clamp(90px, 14vw, 160px) clamp(18px, 6vw, 78px) 82px;
  background:
    linear-gradient(90deg, rgba(10, 9, 7, 0.78) 0%, rgba(10, 9, 7, 0.42) 48%, rgba(10, 9, 7, 0.08) 100%),
    linear-gradient(0deg, rgba(10, 9, 7, 0.64) 0%, rgba(10, 9, 7, 0) 48%),
    url("https://images.unsplash.com/photo-1414235077428-338989a2e8c0?auto=format&fit=crop&w=1800&q=82") center / cover;
  color: var(--white);
}

.hero-copy {
  position: relative;
  z-index: 1;
  max-width: 760px;
}

.eyebrow {
  margin: 0 0 12px;
  color: var(--tomato);
  font-size: 0.78rem;
  font-weight: 900;
  letter-spacing: 0.13em;
  text-transform: uppercase;
}

.hero .eyebrow,
.story .eyebrow {
  color: #f4d58a;
}

h1,
h2,
h3,
p {
  margin-top: 0;
}

h1,
h2 {
  font-family: "Noto Serif SC", Georgia, "Times New Roman", serif;
  font-weight: 600;
  letter-spacing: 0;
}

h1 {
  margin-bottom: 22px;
  font-size: clamp(4.2rem, 11vw, 9rem);
  line-height: 0.92;
}

h2 {
  margin-bottom: 0;
  font-size: clamp(2rem, 4.2vw, 4.4rem);
  line-height: 1.03;
}

h3 {
  margin-bottom: 6px;
  font-size: 1.08rem;
}

.hero-text {
  max-width: 620px;
  color: rgba(255, 253, 248, 0.84);
  font-size: clamp(1rem, 1.7vw, 1.22rem);
  line-height: 1.8;
}

.hero-actions {
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 30px;
}

.hero-note {
  position: absolute;
  right: clamp(18px, 6vw, 78px);
  bottom: 22px;
  left: clamp(18px, 6vw, 78px);
  z-index: 2;
  justify-content: space-between;
  gap: 16px;
  color: rgba(255, 253, 248, 0.74);
  font-size: 0.9rem;
}

.section-wrap {
  padding: clamp(58px, 8vw, 106px) clamp(18px, 5vw, 72px);
}

.intro {
  display: grid;
  grid-template-columns: minmax(0, 0.9fr) minmax(320px, 0.8fr);
  gap: clamp(26px, 6vw, 74px);
  align-items: end;
  border-bottom: 1px solid var(--line);
}

.intro p:not(.eyebrow) {
  margin-bottom: 0;
  color: var(--muted);
  font-size: 1.05rem;
  line-height: 1.85;
}

.section-heading {
  max-width: 780px;
  margin-bottom: 30px;
}

.section-heading.split {
  display: flex;
  max-width: none;
  align-items: end;
  justify-content: space-between;
  gap: 24px;
}

.text-link {
  color: var(--leaf);
  font-weight: 850;
  white-space: nowrap;
}

.dish-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 18px;
}

.dish-card {
  display: grid;
  gap: 16px;
  border: 1px solid var(--line);
  border-radius: 8px;
  padding: 12px;
  background: var(--white);
  box-shadow: 0 14px 42px rgba(37, 30, 20, 0.08);
}

.dish-card img {
  width: 100%;
  aspect-ratio: 4 / 3;
  border-radius: 6px;
  object-fit: cover;
}

.featured-dish {
  transform: translateY(-14px);
  box-shadow: var(--shadow);
}

.dish-info {
  justify-content: space-between;
  gap: 18px;
}

.dish-info p {
  margin-bottom: 0;
  color: var(--muted);
  line-height: 1.55;
}

.dish-info strong {
  color: var(--tomato);
  font-size: 1.05rem;
}

.order-button {
  width: 100%;
  border-color: var(--line);
  background: transparent;
}

.order-button.added {
  border-color: var(--leaf);
  background: var(--leaf);
  color: var(--white);
}

.story {
  display: grid;
  grid-template-columns: minmax(0, 1.08fr) minmax(320px, 0.92fr);
  min-height: 640px;
  background: var(--ink);
  color: var(--white);
}

.story > img {
  width: 100%;
  height: 100%;
  min-height: 430px;
  object-fit: cover;
}

.story-copy {
  align-self: center;
  padding: clamp(38px, 7vw, 78px);
}

.story-copy p:not(.eyebrow) {
  margin-top: 22px;
  color: rgba(255, 253, 248, 0.76);
  font-size: 1.04rem;
  line-height: 1.85;
}

.story-list {
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 30px;
}

.story-list span {
  border: 1px solid rgba(255, 253, 248, 0.2);
  border-radius: 999px;
  padding: 10px 14px;
  color: rgba(255, 253, 248, 0.82);
  font-weight: 750;
}

.reservation-panel {
  display: grid;
  grid-template-columns: minmax(0, 0.78fr) minmax(360px, 0.9fr);
  gap: clamp(26px, 5vw, 56px);
  align-items: start;
  border: 1px solid var(--line);
  border-radius: 8px;
  padding: clamp(24px, 4vw, 46px);
  background: linear-gradient(135deg, var(--white), var(--linen));
}

.reservation-form {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
}

.reservation-form label {
  display: grid;
  gap: 8px;
  color: var(--muted);
  font-weight: 800;
}

.reservation-form input,
.reservation-form select {
  min-width: 0;
  height: 48px;
  border: 1px solid var(--line);
  border-radius: 8px;
  padding: 0 14px;
  background: var(--white);
  color: var(--ink);
}

.reservation-form button,
.form-message {
  grid-column: 1 / -1;
}

.form-message {
  min-height: 24px;
  margin-bottom: 0;
  color: var(--leaf);
  font-weight: 850;
}

.hours {
  display: grid;
  grid-template-columns: minmax(0, 0.65fr) minmax(320px, 1fr);
  gap: clamp(26px, 6vw, 76px);
  padding: clamp(58px, 8vw, 104px) clamp(18px, 5vw, 72px);
  background: #f1eadf;
}

.hours-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 1px;
  overflow: hidden;
  border: 1px solid var(--line);
  border-radius: 8px;
}

.hours-grid div {
  display: grid;
  gap: 8px;
  min-height: 120px;
  align-content: center;
  padding: 22px;
  background: rgba(255, 253, 248, 0.72);
}

.hours-grid span {
  color: var(--muted);
  font-weight: 800;
}

.hours-grid strong {
  font-size: 1.08rem;
}

.wish-drawer {
  position: fixed;
  top: 0;
  right: 0;
  z-index: 50;
  width: min(390px, 100%);
  height: 100%;
  padding: 20px;
  border-left: 1px solid var(--line);
  background: var(--white);
  box-shadow: -20px 0 50px rgba(29, 26, 24, 0.16);
  transform: translateX(100%);
  transition: transform 220ms ease;
}

.wish-drawer.open {
  transform: translateX(0);
}

.drawer-header {
  justify-content: space-between;
  padding-bottom: 16px;
  border-bottom: 1px solid var(--line);
}

.icon-button {
  display: grid;
  width: 40px;
  height: 40px;
  place-items: center;
  border: 1px solid var(--line);
  border-radius: 50%;
  background: var(--white);
  color: var(--ink);
  cursor: pointer;
}

.wish-items {
  display: grid;
  gap: 12px;
  padding-top: 18px;
  color: var(--muted);
}

.wish-item {
  display: flex;
  justify-content: space-between;
  gap: 12px;
  border-bottom: 1px solid var(--line);
  padding-bottom: 12px;
  color: var(--ink);
}

.site-footer {
  justify-content: space-between;
  padding: 28px clamp(18px, 5vw, 72px);
  background: var(--ink);
  color: rgba(255, 253, 248, 0.72);
}

@media (max-width: 980px) {
  .site-header {
    grid-template-columns: 1fr auto;
  }

  .main-nav {
    display: none;
  }

  .intro,
  .story,
  .reservation-panel,
  .hours {
    grid-template-columns: 1fr;
  }

  .dish-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .featured-dish {
    transform: none;
  }
}

@media (max-width: 650px) {
  .site-header {
    padding: 12px 16px;
  }

  .ghost-link {
    display: none;
  }

  h1 {
    font-size: clamp(3.7rem, 17vw, 5.4rem);
  }

  .hero {
    min-height: 760px;
    padding-inline: 16px;
  }

  .hero-note,
  .section-heading.split,
  .site-footer {
    align-items: stretch;
    flex-direction: column;
  }

  .hero-note {
    align-items: flex-start;
  }

  .hero-actions .button,
  .reservation-form button {
    width: 100%;
  }

  .dish-grid,
  .reservation-form,
  .hours-grid {
    grid-template-columns: 1fr;
  }

  .story-copy,
  .section-wrap,
  .hours {
    padding-inline: 16px;
  }
}
</style>
