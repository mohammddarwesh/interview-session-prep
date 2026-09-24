<div dir="rtl" lang="ar">

# Mega Dev — One-Day Cram (React / TypeScript / Tailwind)

**Interview:** Mega Dev — Tanta | **Language:** عربي (Technical terms بالإنجليزي)
**Targets from the JD:** React + TypeScript + Tailwind CSS · Clean Code · Performance & UX · APIs + State Management · Git & GitHub

> **طريقة المذاكرة:** اقرا السؤال الأول، غطّي الإجابة، حاول تجاوب بصوت عالي بالإنجليزي/عربي، وبعدين قارن. الإجابة دي لو حفظتها يبقى انت كسبت.
> **أهم قاعدة في أي إنترفيو React:** اربط جوابك ببروجكت عملته فعلاً (StoreHub / Taskly / RestIQ / Get2Cars) — المُراجع بيحب المثال الواقعي مش النظرية.

---

## 1. React Internals & Re-renders (أهم جزء — أغلب الأسئلة من هنا)

### Q1. إيه الفرق بين الـ Virtual DOM والـ Real DOM؟
**الإجابة:**
- الـ Real DOM ده الـ HTML فعلي في المتصفح. أي تعديل بيعمل reflow + repaint وبيكون مكلف في الـ performance.
- الـ Virtual DOM صورة في الـ memory من الـ UI. لما تعمل `setState`، React بيعمل:
  1. يبني Virtual DOM جديد مقارنة بالقديم.
  2. **reconciliation**: بيقارنهم ببعض (الـ diff) ويحدد التغيير.
  3. بيتعامل بس مع العناصر اللي اتغيرت في الـ real DOM (`reconciliation` + `commit`).
- نتيجة: تعديلات أسرع لأننا مش ماسكيين الـ DOM مباشرة.

### Q2. إمتى React تعمل re-render؟
**الإجابة** (دي سؤال بيوضح فهمك، اذكرهم كلهم):
1. لما يتغير الـ `state` داخل الـ component نفسه.
2. لما يتغير الـ `props` القادمين من الأب (والأب عمل re-render).
3. لما الـ parent يعمل re-render (حتى لو الـ props نفسها — لأنها بتتكوّن جديد).
4. لما `context` بتستعمله يتغير.
5. لما `forceUpdate` يتم استدعاؤه.

**مهم تلحق:** مجرد re-render مش معناها أن الـ DOM هيتغير — يعني React هتشتغل الـ diff وتقرر. و re-render مش re-mount.

### Q3. إيه هو الـ `key`؟ ليه مهم؟
- الـ `key` بيساعد React في الـ reconciliation أنها تعرف مين العنصر اللي اتغير داخل القايمة.
- من غيره React بتستخدم الـ index — وده بيسبب مشاكل لو بتضيف/تمسح عناصر في النص (state خاطئ، input بيحتفظ بقيمة غلط).
- **قاعدة:** استخدم ID حقيقي من الـ data، ومتستخدمش الـ index غير لو القايمة static ومش هتتغير.

### Q4. `useMemo` vs `useCallback` vs `memo` — متى أستعمل مين؟
- `useMemo` → بيحفظ **قيمة محسوبة** (result) — بيستخدمه لو الحساب غالي: `const total = useMemo(() => items.reduce(...), [items])`.
- `useCallback` → بيحفظ **الدالة نفسها** بالـ reference — لو بتعدي دالة لـ child معمول له `memo` عشان الـ child ميفضلش re-render.
- `memo()` → بتلف الـ component كامل، فميعملش re-render غير لو الـ props اتغيرت فعلاً.
- **تحذير للإنترفيو:** "المفروض أستخدمهم بوعي مش في كل حتة — أبدأ من غيرهم وأضيفهم بس لما ألاحظ مشكلة أداء، لأن الـ memoization نفسه له overhead في الـ memory والـ comparison."

### Q5. إيه إعادة الـ reconciliation فعلياً؟ (بيشتغل إزاي)
- React بيكوّن tree جديد من العناصر، وبيقارن بالـ previous tree بـ **heuristic** (مش مقارنة كاملة):
  - لو نوع الـ element اتغير (`<div>` → `<span>`) → React بتموت الـ subtree القديمة وتبني جديد (remount).
  - لو نفس النوع → بتقارن الـ props وبتحدث اللي اتغير (update).
  - اللوب بتتباني بـ `key`.
- الطفل بيرجع نفس الـ type →React بتحتفظ بـ state الـ component.

### Q6. الفرق بين `controlled` و `uncontrolled` components؟
- **Controlled:** القيمة متحكم فيها React — `value` + `onChange` دايماً مرتبطين ببعض، الـ React هي مصدر الحقيقة الوحيد.
- **Uncontrolled:** بيستخدم `ref` عشان يقرا القيمة (مشكلتنا احنا بنسيطر على الـ DOM نفسه).
- **في الإجابة أضيف:** "بفضل controlled في المشاريع مع React Hook Form + Zod لأن الـ validation والـ state كلهم في مكان واحد."

### Q7. إيه معنى "إزاي تمنع re-render غير ضروري"؟
- `React.memo` علىـ child-heavy components.
- `useCallback` للفانكشنز المعديّة لـ memoized children.
- رفع الـ state: لو جزء من الـ UI مش محتاج الـ state، خليه component لوحده (لو الـ state اتغير، بس ده اللي re-renders).
- **Composition:** مرر الـ children بدل الـ props الفردية عشان مفيش re-render لما الـ parent يتغير.
- React Query/Zustand بتقلل re-renders بدل ما ترفع كل حاجة على الـ Context.

### Q8. ليه بنستدعي hooks جوه component بس؟ (Rules of Hooks)
- الـ hooks بتنادي على الـ **fiber node / hook list** بالترتيب في كل render. لو الـ order اتغير (جوه if/loop) → الـ state هيتلخبط.
- فلازم دايماً في الـ top level، وممنوع في `if`/`for`/الدوال العادية غير الـ components (أو custom hooks).

### Q9. سؤال عملي (بتحبوه في الشركات): ليه `useEffect` بيعمل double-invoke في StrictMode؟ وليه `useEffect` مش للـ rendering؟
- StrictMode بيعمل mount → unmount → mount عشان يفضح الآثار الجانبية — بيانات تطويرية بس، مش هتظهر في الـ production build.
- الـ `useEffect` بيشتغل **بعد** الـ paint — عشان كده مش مناسب للـ layout مبيتكلم بـ"render ليه" بل "أثر جانبي حصل إيه بعد ما الـ UI ظهر".

---

## 2. JavaScript (أهم 12 سؤال — أساس أي سؤال React)

### Q1. إيه هو الـ Closure؟ (السؤال رقم 1 في العالم)
- الدالة الداخلية بتفضل فاكرة الـ scope اللي اتولدت فيه حتى بعد ما الدالة الأب تخلص.
```js
function counter() {
  let count = 0;
  return () => ++count;   // closure: فاكرة count
}
const c = counter();
c(); c(); // 1, 2
```
- **مثال الإنترفيو الشهير:**
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Output: 3 3 3  (var scope function-level، كل الكولباك بيشارك نفس i)
```
- **الحل:** `let` (يتعمل لكل iteration variable جديدة) أو `IIFE` أو `forEach`.
- **اربطها بالـ React:** الـ stale closure — لو closure فاكر قيمة قديمة من الـ state (مشكلة في `useEffect` لما الـ deps غلط).

### Q2. إيه ترتيب الـ Event Loop؟ (أكيد هيجيبها)
`Sync code → Microtasks (Promises) → Macrotasks (setTimeout/setInterval)`
```js
console.log("A");                                 // sync
Promise.resolve().then(() => console.log("B"));   // microtask
setTimeout(() => console.log("C"), 0);            // macrotask
console.log("D");                                 // sync
// Output: A D B C
```
- ملحوظة مهمة: الـ render للصفحة بيحصل بين الـ microtasks والـ macrotasks.

### Q3. إيه الفرق بين Promise.all و allSettled و race؟
- `Promise.all` → **يفشل كله أول ما واحد يفشل** (short-circuit). لو محتاج كل الـ requests تكمل بنجاح.
- `Promise.allSettled` → بيستنى الجميع، وغالباً بيرجع `{status: "fulfilled" | "rejected"}` و**مش بيفشل أبداً** — مثالي لو كل طلب مستقل.
- `Promise.race` → أول واحد يخلص (أياً كان نجاح أو فشل) — للأداء والـ timeout.

### Q4. ليه نلف الـ `setTimeout` في Promise؟ وإيه اللي بيسمعك إنه sync؟
- عشان نقدر نعمل `await` على حاجة async قديمة (callback-based).
```js
const wait = (ms) => new Promise(res => setTimeout(res, ms));
```
- **مفاجأة الإنترفيو:** الـ Promise executor بيتنفذ **sync فوراً**، و `resolve()` مبوقّفش الكود:
```js
const p = new Promise((resolve) => {
  console.log("A"); resolve("B"); console.log("C");
});
p.then(v => console.log(v));
console.log("D");
// Output: A C D B
```

### Q5. `==` vs `===` مع أمثلة coercion؟
- `===` → يقارن النوع والقيمة من غير تحويل. **دي الصح دايماً.**
- `==` → بيعمل type coercion وده سبب أخطاء مشهورة:
```js
0 == ""         // true
0 == false      // true
[] == false     // true   ([] → "" → 0)
null == undefined // true
"5" + 3         // "53" (string)
"5" - 3         // 2    (number)
```

### Q6. الـ Hoisting و TDZ؟ `var` vs `let` vs `const`؟
- `var` → بيتهوّست وبيبقى `undefined` (function-scoped).
- `let`/`const` → بيتهوّست بس في **Temporal Dead Zone (TDZ)** — لو دوصت عليه قبل تعريفه → `ReferenceError`. (block-scoped).
- `const` → لازم يتعرف بقيمة، ومينفعش إعادة تخصيص (بس الـ object جواه متاح للتعديل).

### Q7. `this` بيتحدد إزاي؟ وإيه دور الـ arrow function؟
- الـ `this` بيتحدد **من طريقة الاستدعاء** مش من مكان التعريف (regular functions).
- الـ arrow function مالهاش `this` خاص بيه — بياخده من الـ scope الأب (بالتالي مثالي جوه React class/component methods).
- أمثلة: `obj.method()` → obj. ودالة لوحدها → global. و `new F()` → object جديد.

### Q8. `call` / `apply` / `bind` — الفرق؟
- التلاتة بيعدّلوا الـ `this` بتاع دالة:
  - `call(thisArg, a, b)` → ينادي على طول.
  - `apply(thisArg, [a, b])` → نفس الكلام بس بحجج كـ array.
  - `bind(thisArg)` → بيرجع **دالة جديدة** مربوطة — تشتغل عليها وانت مستريح (مهم في الـ event handlers).

### Q9. `map` vs `forEach` vs `reduce` — إمتى مين؟
- `map` → **بيرجع array جديد** بنفس الطول (تحويل) — **الأفضل** في الـ React للرندر.
- `forEach` → بيعمل loop و**بيرجع nothing** — للـ side effects بس.
- `reduce` → بيرجع **قيمة واحدة مجمّعة** من محتوى array: `[1,2,3].reduce((a,b) => a+b, 0) // 6`
- **مثال إنترفيو:** عد العناصر المتكررة أو `flatten` مع reduce.

### Q10. Shallow vs Deep copy؟ (معروف كويس)
- `const b = a` → بنسخ الـ **reference** مش الـ array (أي تعديل بيعدل على الأصل).
- **Shallow:** `[...arr]` / `{...obj}` — بيتنسخ الـ nested objects بالـ reference.
- **Deep:** `structuredClone(obj)` (native) أو `JSON.parse(JSON.stringify(obj))` (بيسقط functions/dates).
- **اربطها بالـ React:** من أهم قواعد الـ immutability — `setState` بالcopy مش بالتي mutate.

### Q11. Debounce vs Throttle؟ (مهمة لأداء الـ inputs)
- **Debounce:** ييجي **بعد** ما المستخدم يقف الكتابة (مثلاً 300ms) — للـ search inputs. بنأجل التنفيذ حتى يمر وقت خالي من الأحداث.
- **Throttle:** يشتغل **مرة لكل فترة** ثابتة مهما كترت الأحداث — للـ scroll/resize.
- **في المشروع:** الاستدعاء الـ debounce في Taskly للبحث المتزامن مع URL.

### Q12. Optional chaining `?.` و Nullish coalescing `??`؟
- `user?.profile?.name` → بيرجع `undefined` بأمان لو أي جزء `null/undefined` (مش يرمي error).
- `0 || 10` → `10` بينما `0 ?? 10` → `0` (لأن `??` بيبص على `null/undefined` بس مش falsy).
- **في React:** بتستخدمهم كتير مع الـ API data وقت الـ loading.

---

## 3. TypeScript (أهم 6 أسئلة)

### Q1. `interface` vs `type` — الفرق؟
- الاتنين بيعملوا نفس الغرض، بس فرقين جوهريين:
  - `interface` بيتدمج مع بعضه (declaration merging) وبيتوسع بـ `extends`.
  - `type` بيقدر يعمل **unions** / **intersections** / **tuples** / mapped types: `type Status = "idle" | "loading" | "success"`.
- **القاعدة:** لو object/class → `interface`. لو union/مشتقات → `type`.

### Q2. أذكر 5 Utility Types وكل واحد بيعمل إيه؟
- `Partial<T>` → كل الخصائص اختيارية.
- `Required<T>` → كل الخصائص مطلوبة.
- `Pick<T, K>` → اختر خصائص معينة.
- `Omit<T, K>` → احذف خصائص معينة.
- `Readonly<T>` → كل الخصائص للقراءة بس.
- `Record<K, V>` → object بمفاتيح من النوع `K` وقيم `V`.
- `ReturnType<T>` → نوع ما بترجّعه دالة.

### Q3. Generics — عمل إيه؟ مثال بسيط؟
- بيسمحلك تفضل الـ type مرتبط ببعضه بدون ما تحدد قيمته.
```ts
function identity<T>(arg: T): T { return arg; }
```
- مثال حقيقي: `useQuery<T>(...)` — بنقول لـ React Query إيه نوع الداتا اللي هترجع، والـ data نوعها معروف في كل المشروع.

### Q4. Type narrowing — أمثلة؟
- `typeof` / `instanceof` / `in` operator / discriminated unions.
```ts
type Res = { ok: true; data: User } | { ok: false; error: string };
function handle(r: Res) {
  if (r.ok) console.log(r.data);   // ts يعرف ان هنا data موجودة
  else console.log(r.error);
}
```

### Q5. إزاي تعرف نوع الـ API response في TypeScript؟
- نعرّف `interface UserResponse { id: number; name: string }`
- و `axios.get<UserResponse>(...)` أو `await res.json() as UserResponse` ويفضل `zod` validation.
- **إضافة قوية للإنترفيو:** "بحب أستخدم Zod مع TypeScript — لأن الـ types بتقفل وقت الـ compile بس، والـ data من الـ API difference نوعها runtime. الزود بيضمن الاثنين."

### Q6. `any` vs `unknown` — ليه `any` وحش؟
- `any` بيبطل الـ type checking خالص (الـ TS بيكفيه) → سرعة في تسريب الـ errors.
- `unknown` بيقول الـ TS: "مش عارف نوعه، وحط شرط قبل ما تستعمله" → بعمل narrowing الأول ثم استخدمه.
- القاعدة: بلاش `any` في الكود؛ `unknown` ثم narrowing.

---

## 4. State Management (أهم 5 أسئلة)

### Q1. مين إحنا محتاجين State Management أصلاً؟ ولما Context لوحده مش كفاية؟
- لو الـ state اتشارك جوه شجرة كبيرة وبيتغير كتير، Context هيعمل re-render لكل الـ consumers في كل تغيير — مش optimized.
- Context مناسب للـ static-ish زي theme/auth/toast. غير كده بتحتاج Redux Toolkit أو Zustand (بيشتغلوا بـ subscribers فيمنعوا re-render الغير ضروري).

### Q2. Redux Toolkit vs Zustand — مين ومن إمتى؟
- **Redux Toolkit:** أكتر تنظيماً (slices, thunks, devtools), مناسب لمشاريع كبيرة وفرق كبار.
- **Zustand:** أبسط، وقليل من الـ boilerplate، بيشتغل بـ `useStore` والـ selectors بيحددوا مين اللي re-renders.

### Q3. إيه الفرق بين الـ "client state" والـ "server state"؟ وليه بنخليهم مختلفين؟
- **Client state:** UI, session, form, dark mode — بتعيش في المتصفح، ملك التطبيق.
- **Server state:** البيانات اللي جاية من الـ API (users, orders) — ليها cache, stale, invalidations, refetch.
- **قاعدتي في المشاريع:** Redux/Zustand للـ client state، و TanStack Query للـ server state — عشان الـ query بيعمل caching, background refetch, optimistic updates بالشكل الصح.

### Q4. إيه الـ optimistic update في TanStack Query؟
- بنحدّث الـ cache فوراً قبل ما السيرفر يرد (UI بيحس إنه instant).
- بنرسل الـ mutation، لو فشلت → بنرجع الـ cache القديم (rollback) ونعرض error.
- مثال: دق الـ drag-and-drop على الـ Kanban في Taskly.

### Q5. Context + useReducer بيتحلوا محل Redux؟
- ممكن لمشاريع صغيرة، بس ضعيف أداءً ويفتقر **middleware / devtools / selectors / time-travel debugging** وبيخلي كل المستخدمين يعيدوا render على أي قيمة بديلة.

---

## 5. Performance & Clean Code (أهم 6 أسئلة)

### Q1. إزاي بتحسّن Performance في React app؟
**قائمة جاهزة للإجابة (اربطها بمشاريعك):**
1. **Code splitting + React.lazy/Suspense** — مش بننزّل كل الباندل زي ما هو.
2. **TanStack Query caching** — بنمنع requests متكررة.
3. **`React.memo` + `useMemo` + `useCallback`** — نمنع re-renders.
4. **Image optimization** — `next/image` / lazy loading.
5. **Vendor splitting** في bundler config.
6. **Debounce** في الـ search.
7. **Avoid rendering heavy lists** → virtualized lists (لو فيه قايمة طويلة).

**للاستدلال الحقيقي:** "في Get2Cars رفعت Lighthouse من 60 لـ 90+ بكود splitting + caching."

### Q2. إيه الفرق بين `useEffect` كأداة تجلب بيانات و React Query؟
- `useEffect` لـ fetch → بتحتاج تحدد حالات pending/error بإيدك، وبتحبس fetch متكرر، وشفرة كبيرة.
- React Query → cache + refetch + retry متضمنة وبيجلو شفرة أقصر.

### Q3. Clean Code — إللي معناه في React؟
- **Components صغيرة - single responsibility** (مش component 500 سطر).
- **Naming** واضح: `useGetOrders`, `fetchOrders`, handleX = event.
- **DRY:** كرر أدوات مش كود — زي الـ API layer بتاعك في Get2Cars (5 generic hooks) والـ 9-file CRUD template.
- **صفر side effects** جوه render؛ كل الآثار في effects/events.
- **TypeScript كامل** بدل `any`.
- **Refactor بالمراحل:** لو الوظيفة تسير عمل تنقص، لا تسير وتعمل refactor.

### Q4. إيه هي الـ "reusable custom hook" وعاملتها إزاي؟
- دالة اسمها يبدأ بـ `use` بتلف منطق متكرر. مثال حقيقي:
  - `useItems` → Get/Put/Delete مع الـ loading/error/refresh.
  - هوك الـ infinite scroll/pagination الموحد في Taskly.
- **القيمة:** اختزال تكرار (3 مرات، ثم ملاحظة).

### Q5. ليه بُعد الكود (`code splitting`) مهم لـ UX؟
- أول باندل أصغر ⇒ أول load أسرع ⇒ المستخدم يرى الواجهة أسرع. الأجزاء الغير مستخدمة بتتحميل عند الحاجة (lazy / route-based).

### Q6. إيه الـ Accessibility / UX اللي بتخليك متميز؟
- Semantic HTML (button مش div قابلة للكزة).
- Focus states مرئية، `aria-label` على الـ icons.
- Empty states والـ loading skeletons (مش spinner طول اليوم).
- Validation رسائل واضحة في الفورم.
- Performance كجزء من UX (استجابة سريعة ⇒ رضا المستخدم).

---

## 6. Tailwind CSS (متوقعة لأن في الـ JD)

### Q1. ليه Tailwind وليس CSS classes عادية / CSS Modules؟
- الـ utility classes بتقلل تكرار الـ CSS وبتفرض منحنيات متناسقة (spacing, colors من الـ config مش ابداع حر).
- لا يزول من الـ CSS الذي لا يُستخدم (tree-shaking) ⇒ باندل أصغر.
- Responsive مباشر من غير media queries منفصلة: `md:flex`.

### Q2. إيه عيوبه/متى متستخدمهوش؟
- الـ JSX ممكن يضخم من inline classes — بنعمل components متخصصة (Button, Card) عشان النضف.
- لو تسوي Design System بين Teams قوية، `Tailwind + shadcn/ui` جزء من الحلقة.

---

## 7. Git & GitHub (أسئلة مباشرة محتملة)

### Q1. إيه الفرق بين merge و rebase؟
- **merge** → commit جديد يدمج التفرع، بيحفظ full history (سايف).
- **rebase** → يعيد كرو السيولة فوق الـ branch target عشان history نضيف خطي، لكنه يعيد كتابة commits (خطرة على الـ public branches).
- **خلاصة:** اعمل rebase محل محلياً، وmerge عند الـ shared.

### Q2. إيه الـ cherry-pick؟
- ينقل commit معين من برانش لبرانش من غير ما يسحب البرانش كله.
- مثال: hotfix من main لـ release.

### Q3. في migrate مشكلة، أشتغل إيه؟
- `git status` → شوف الحريق، راجع `git diff` بشأن ما تغيّر، لو تريد تراجع: `git checkout -- <file>` → `git reset` → revert commit بـ `git revert`.
- **قاعدة:** `git revert = commit جديد يعكس التغيير` (أمان في الـ shared)، `git reset --hard = يمسح تاريخ` (شمال للمحلي).

### Q4. Git flow ومعهود الفرق في شركات؟
- branch: `main` + `develop` + `feature/xxx` + `hotfix/xxx` → PRs مع review → merge بعد approval.
- Conventional commits: `feat:`, `fix:`, `chore:`.

### Q5. إزاي تعمل الـ PR وبتحميه؟
- `git checkout -b feature/xxx` → اعمل commits → `git push -u origin feature/xxx` → افتح PR → راجع diff بنفسك → اطلب review → سيب أتمنة على “squash and merge”.

---

## 8. أسئلة HR تخص Mega Dev (لأن الـ JD مكتوب بعامية مضحكة و "عينك على التفاصيل")

### Q1. عرّفنا بنفسك (دقيقة واحد)
> "أنا مطور Frontend خبرة سنتين في React و TypeScript. اشتغلت في Kits Technology على منصة عربية للعربيات، كنت مسؤول عن ربط 38 API endpoint وعمل RBAC وتحسين الأداء من 60 لـ 90+ في Lighthouse. وبجوار شغل في الـ Freelance بنيت مشاريع full-stack زي منصة تجارة إلكترونية بـ Stripe وتطبيق إدارة مهام بـ optimistic updates. بحب الاهتمام بالتفاصيل وبتحويل الأفكار لكود نظيف سريع، وبستمتع بالشغل وسط تيم."

### Q2. ليه عايز تشتغل عندنا؟
> "شفت إن Mega Dev في طنطا وبتحب التحول لأكودة وشغّالين في تطبيقات ويب، وعجبني إن الـ stack بتاعكم React/TypeScript هو نفس اللي شغال بيه، فمش هحتاج فترة adaptation وأقدر أضيف فوراً. وباشتغل كويس لما أكون جوه تيم وبتعلم منهم." *(ابحث قبل الإنترفيو في Mega Dev على Google/LinkedIn وحط تفصيلة حقيقية واحدة — "شفت إنكم عاملين X")*

### Q3. نقطة قوة ونقطة ضعف؟
- **قوة:** `ownership` — بسند ميزة من التصميم للـ deployment، زي الـ API layer اللي أسسته وأه كان قياس لكل API call في البروجكت.
- **ضعف:** "بقالي فترة بميل أعمل optimize قبل ما أبعت أول نسخة. اتعلمت أشتغل بالأبسيط الأول وأعمل refactor بعد ما الـ feature تسير — دلوقتي بحدد وقت للتحسين بعد الشغل."

### Q4. راتبك المتوقع؟
- "حسب الشغل والمستوى، أنا واثق من إمكانياتي في React و TypeScript والمشاريع اللي عملتها. عايز رقم عادل للسوق في طنطا — فيه budget محدد للدور؟"

### Q5. إيه أكبر إنجاز ليك (STAR)?
> "في Get2Cars، الداشبورد كان بطيء — Lighthouse حوالي 60. حللت الـ bundle وقسمته بـ React.lazy وسويت caching بـ TanStack Query، وبعدها وصل لـ 90+. العميل كان مبسوط والطريقة بقت standard لكل المزايا الجديدة."

### Q6. سؤال للـ interviewer في الآخر (مهم جداً — اسأله سؤالين):
- "طيب الإيكونومي، الـ workflow عندكم بيمشي إزاي — Scrum ولا Kanban؟"
- "إيه أكبر تحدّي الـ Frontend team بيمر به دلوقتي؟"
- "بتعملوا Code review قبل ما الـ code يطلع؟"

---

## 9. مخطط اليوم (لو حابب تتبع)

| الوقت | الحتة | المدّة |
|-------|-------|--------|
| 1 | راجع القسم 1 (React) وشيل الأخطاء | 90 دقيقة |
| 2 | القسم 2 (JS) سريع ثم 3 + 4 (TypeScript + State) | 90 دقيقة |
| 3 | القسم 5 + 6 (Performance + Tailwind) | 60 دقيقة |
| 4 | القسم 7 + 8 (Git + HR) | 60 دقيقة |
| 5 | **Mock interview معايا (حوالي 30–45 دقيقة)** | بعد ما تخلص |

</div>