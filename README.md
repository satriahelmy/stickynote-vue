# 🗒️ Memo Board — Vue 3 Single-File Component

A minimal Vue 3 Single-File Component (SFC) to create, display, and delete colorful memos directly in the browser. Built with the Composition API and `<script setup>` syntax.

---

## ✨ Features

- Add memos via a modal (textarea + **Save** button)
- Delete individual memos (tiny **X** button on each card)
- Random background color for every new memo
- Automatic date using `toLocaleDateString()`
- Clean, lightweight CSS styles

---

## 🧰 Tech & Prerequisites

- **Vue 3** + **Composition API** (`<script setup>`)
- Recommended bundler: **Vite**
- **Node.js** 18+ and **npm**/**yarn**/**pnpm**

---

## 🚀 Getting Started (with Vite)

1) Create a new Vue project (optional if you already have one):
```bash
# choose one
npm create vite@latest stickynote-vue -- --template vue
# or
pnpm create vite stickynote-vue --template vue
# or
yarn create vite stickynote-vue --template vue
```

2) Install dependencies and enter the project:
```bash
cd stickynote-vue
npm install
```

3) Create the component file at `src/components/MemoBoard.vue` and paste your code:
```vue
<script setup>
import { ref } from 'vue';

const showForm = ref(false);
const newMemo = ref('');
const memos = ref([]);
const errorMessage = ref('');

function addMemo() {
  memos.value.push({
    id: Date.now(),
    memo: newMemo.value,
    date: new Date().toLocaleDateString(),
    backgroundColor: `#${Math.floor(Math.random()*16777215).toString(16)}`
  });

  newMemo.value = '';
  showForm.value = false;
}

function deleteMemo(id) {
  memos.value = memos.value.filter(memo => memo.id !== id);
}
</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo</h1>
        <button class="header-button" @click="showForm = true">+</button>
      </header>

      <div class="card-container">
        <div
          v-for="memo in memos"
          :key="memo.id"
          class="card"
          :style="{ backgroundColor: memo.backgroundColor }"
        >
          <p class="card-content">{{ memo.memo }}</p>
          <div class="card-footer">
            <p class="card-date">{{ memo.date }}</p>
            <button @click="deleteMemo(memo.id)" class="card-button">X</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <button @click="showForm = false" class="form-close-btn">&times;</button>
        <textarea v-model="newMemo" name="memo" id="memo" cols="30" rows="10" class="form-textarea"></textarea>
        <button @click="addMemo" class="form-save-btn">Save</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
/* Styles trimmed here for brevity; paste yours in full */
</style>
```

4) Use the component in `src/App.vue`:
```vue
<script setup>
import MemoBoard from './components/MemoBoard.vue'
</script>

<template>
  <MemoBoard />
</template>
```

5) Run the dev server:
```bash
npm run dev
```

---

## 🧩 Component Overview

### Reactive State
- `showForm: boolean` — controls the visibility of the add-memo modal
- `newMemo: string` — the textarea value for a new memo
- `memos: Array<{ id, memo, date, backgroundColor }>` — the list of memo cards
- `errorMessage: string` — available if you want to add validation (optional)

### Methods
- `addMemo()` — pushes a new memo into `memos`, sets a random color, assigns the current date, then clears the input and closes the modal
- `deleteMemo(id)` — removes a memo by its `id`

### Template & Styles
- Header with a **+** button to open the modal
- Memo grid using `v-for` over `memos`
- Modal (`v-if="showForm"`) with textarea and **Save** button
- Simple CSS for cards, modal, and layout

---

## 🛠️ Customization & Enhancements (Optional)

### 1) Ensure 6-digit HEX colors
`toString(16)` can produce fewer than 6 characters. Pad with zeros:
```js
const hex = Math.floor(Math.random() * 16777215).toString(16).padStart(6, '0');
const color = `#${hex}`;
```

Use it in `addMemo()`:
```js
backgroundColor: `#${Math.floor(Math.random()*16777215).toString(16).padStart(6, '0')}`
```

### 2) Prevent empty memos
Add simple validation and show an error message:
```js
if (!newMemo.value.trim()) {
  errorMessage.value = 'Please enter a memo.';
  return;
}
```
Then in the template:
```html
<p v-if="errorMessage" class="form-error">{{ errorMessage }}</p>
<textarea v-model="newMemo" @input="errorMessage = ''" ...></textarea>
```

### 3) Persist to `localStorage`
Keep memos on page reload:
```js
import { onMounted, watch } from 'vue';

onMounted(() => {
  const saved = localStorage.getItem('memos');
  if (saved) memos.value = JSON.parse(saved);
});

watch(memos, (val) => {
  localStorage.setItem('memos', JSON.stringify(val));
}, { deep: true });
```

### 4) Stable full-screen overlay
Use `position: fixed;` instead of `absolute;` on the overlay to guarantee full-screen coverage:
```css
.form-overlay { position: fixed; /* ...rest */ }
```

### 5) Consistent, locale-aware dates
```js
const date = new Intl.DateTimeFormat('en-US', {
  day: '2-digit', month: '2-digit', year: 'numeric'
}).format(new Date());
```

### 6) UX niceties
- Close modal when clicking the backdrop
- Submit with `Cmd/Ctrl + Enter`
- Truncate very long text with CSS line-clamp
- Ask for confirmation before deleting a memo

---

## 🧪 Manual Test Checklist

- Click **+** → modal appears
- Type memo → click **Save** → new card appears with a background color and date
- Click **X** on a card → the card is removed
- (If validation enabled) Try saving with an empty textarea → see error message
- (If `localStorage` enabled) Refresh the page → memos persist

---

## ❓ Troubleshooting

- **Modal opens but memo isn’t added**  
  Ensure the **Save** button calls `addMemo` and the textarea uses `v-model="newMemo"`.
- **Invalid color codes sometimes**  
  Use `.padStart(6, '0')` on the hex string.
- **Error message shows up too early**  
  Reset `errorMessage` when opening the modal, or clear it on input.

---

## 📄 License

Free for learning and personal use. Add a license file (e.g., MIT) if you plan to publish or distribute.
