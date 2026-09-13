# Frontend Interview Prep — React / Next.js

---

## Overview
- **Start Date:** 2026-08-16
- **Timeline:** 3 weeks
- **Goal:** Land a frontend developer role (React/Next.js)
- **Level:** Advanced — refreshing forgotten concepts
- **DSA Level:** Beginner — coding practice is JS-focused, not algorithm-heavy

---

## Menu
1. [Weekly Plan](#weekly-plan)
2. [Session Structure](#session-structure)
3. [Progress Tracker](#progress-tracker)
4. [Project Deep-Dive](#project-deep-dive)
5. [HR / Personal Questions](#hr--personal-questions) — 33 questions with personalized answers
6. [Session Summaries](#session-summaries)

---

## Weekly Plan

### Week 1 — Foundations + JavaScript
| Day | Topic | Status |
|-----|-------|--------|
| 1 | Closures, `this`, hoisting, scope, prototypes, `==` vs `===` | Done |
| 2 | Event loop, microtasks/macrotasks, promises, async/await, error handling | Done |
| 3 | ES6+: destructuring, spread, optional chaining, map/filter/reduce, Set/Map | Pending |
| 4 | TypeScript: types/interfaces, generics, utility types, narrowing | Pending |
| 5 | HTML/CSS: flexbox, grid, specificity, responsive, box model | Pending |
| 6 | Practice: 8–10 JS coding problems | Pending |
| 7 | Review + mock yourself on Week 1 | Pending |

### Week 2 — React Deep Dive
| Day | Topic | Status |
|-----|-------|--------|
| 8 | Component lifecycle, props/state, re-renders, key, controlled vs uncontrolled | Pending |
| 9 | Hooks deep: useState, useEffect, useRef, custom hooks, rules of hooks | Pending |
| 10 | useMemo/useCallback, reconciliation, virtual DOM, memo, re-renders | Pending |
| 11 | Context API, Redux Toolkit/Zustand, React Query | Pending |
| 12 | Performance: code splitting, lazy loading, error boundaries, portals | Pending |
| 13 | Practice: build TODO app + 5 React coding questions | Pending |
| 14 | Review + drill weak React concepts | Pending |

### Week 3 — Next.js + Interviews
| Day | Topic | Status |
|-----|-------|--------|
| 15 | Next.js App Router: routing, layouts, pages, dynamic routes | Pending |
| 16 | Data fetching: RSC, client/server components, caching, revalidation | Pending |
| 17 | SSR/SSG/ISR/CSR + Pages Router legacy | Pending |
| 18 | Next.js features: middleware, images, metadata, auth | Pending |
| 19 | Build: mini Next.js project | Pending |
| 20 | Mock interview #1 (Technical) | Pending |
| 21 | Mock interview #2 (HR / Behavioral) | Pending |

---

## Session Structure

**Daily rhythm — 2 sessions per day:**

### Session A — Technical (JS + React/Next.js)
- JavaScript fundamentals quiz
- React / Next.js deep-dive questions
- Live coding problems (JS logic, not DSA)
- Project-specific technical questions

### Session B — HR / Personal (behavioral + soft skills)
- Practice 2-3 HR questions per session
- Project walkthroughs using STAR method
- "Tell me about yourself" refinement
- Strengths / weaknesses / conflict questions

---

## Progress Tracker

| Day | Score | Gaps Found | Completed |
|-----|-------|------------|-----------|
| 1 | 7/10 | block vs function scope, promise wrapping, null vs undefined, map return type | Done |
| 2 | 6/14 | Promise executor sync, resolve doesn't stop execution, Promise.all short-circuits | Done |
| 3 | — | — | — |
| 4 | — | — | — |
| 5 | — | — | — |
| 6 | — | — | — |
| 7 | — | — | — |
| 8 | — | — | — |
| 9 | — | — | — |
| 10 | — | — | — |
| 11 | — | — | — |
| 12 | — | — | — |
| 13 | — | — | — |
| 14 | — | — | — |
| 15 | — | — | — |
| 16 | — | — | — |
| 17 | — | — | — |
| 18 | — | — | — |
| 19 | — | — | — |
| 20 | — | — | — |
| 21 | — | — | — |

---

## Project Deep-Dive

### 1. StoreHub — Multi-Tenant E-Commerce SaaS
**Stack:** Next.js App Router, TypeScript, Prisma/MySQL, Clerk, Stripe, TanStack Table, Tailwind/shadcn

**Key talking points:**
- **Multi-tenancy:** Every store-scoped page enforces ownership against Clerk's `userId`. 12 store-scoped API route files. Data isolation by design.
- **Stripe payments:** Checkout creates unpaid order → Stripe session keyed by `orderId` → webhook verifies `Stripe-Signature` → marks order paid. Async reconciliation.
- **31 REST endpoints** across 14 route files. 2 public (checkout + webhook), 29 Clerk-protected.
- **9-file CRUD template** reused for 5 resources (billboards, categories, sizes, colors, products).
- **RSC architecture:** 16 pages fetch data server-side via Prisma singleton. Forms/tables stay client-side. `router.refresh()` re-validates after mutations.
- **Lighthouse:** Admin 99, Storefront 97, SEO 100.

**Common interview questions:**
- How does multi-tenancy work? → Clerk userId ownership check on every store-scoped route
- How do you handle Stripe payments? → Checkout → unpaid order → session → webhook → paid
- Why RSC? → Heavy queries out of client bundles, better performance
- How do you handle auth? → 3-layer: Clerk middleware → server layouts → API handlers

---

### 2. Taskly — Task & Project Management
**Stack:** Next.js App Router, TypeScript, Supabase, Redux Toolkit, TanStack Query, dnd-kit

**Key talking points:**
- **BFF pattern:** 22 endpoints proxy 22 Supabase endpoints. Credentials stay server-side.
- **Full auth lifecycle:** Sign-up, login, logout, silent JWT refresh, password recovery/reset. 7 API route handlers with httpOnly cookies.
- **Edge middleware:** 12 protected routes. Silent JWT refresh with 5-second timeout before rendering.
- **Optimistic Kanban:** dnd-kit drag-and-drop → updates UI instantly → syncs TanStack Query cache → rolls back on failure. Per-column infinite scroll.
- **Responsive lists:** One hook switches between infinite scroll (mobile) and paginated controls (desktop). URL-synced search with debounced queries.
- **State split:** Redux Toolkit = client state (UI, session). TanStack Query = server state (API data, cache).
- **Lighthouse:** Performance 96, Accessibility 98.

**Common interview questions:**
- Why BFF over direct Supabase calls? → Security: credentials stay server-side, controlled API surface
- How does optimistic update work? → Update cache immediately → mutate → if fail, rollback cache
- How do you handle auth? → httpOnly cookies + edge middleware + silent refresh
- Redux vs TanStack Query? → Redux = client state (UI, session). TanStack Query = server state (API, cache)

---

### 3. RestIQ — Restaurant Ordering System
**Stack:** Next.js App Router, TypeScript, Prisma/PostgreSQL, Stripe Elements, NextAuth, Zustand, TanStack Query

**Key talking points:**
- **Full-stack monolith:** Storefront + admin in one deployable. 10 business features, 10 page routes.
- **Order-to-payment pipeline:** Cart checkout → order created → Stripe PaymentIntent server-side → `intent_id` persisted with unique constraint → Stripe Elements on client → success page advances status.
- **RBAC:** Stateless JWT with `isAdmin` claim. 6 session-required routes (401), 2 admin-only mutations (403). Enforced on UI + API.
- **7-model Prisma schema:** DB-enforced integrity (unique slug, unique intent_id, Decimal pricing). 4 versioned migrations + seed pipeline.
- **Hydration-safe cart:** Zustand + localStorage with `skipHydration`. Client rehydrates on mount to avoid SSR mismatch.

**Common interview questions:**
- How does the payment flow work? → Checkout → PaymentIntent → Elements → success page
- How do you prevent SSR hydration mismatch? → `skipHydration` on Zustand, rehydrate on mount, `isMounted` checks
- How does RBAC work? → JWT `isAdmin` claim, enforced server-side (401/403) and client-side (hidden UI)

---

### 4. Get2Cars — Car Rental Platform
**Stack:** React 18, TypeScript, Vite, TanStack Table, React Hook Form, Zod, Recharts, Tailwind

**Key talking points:**
- **38 REST endpoints** through single Axios client with request/response interceptors.
- **5 generic request hooks** (get/post/put/delete/external-get). Pages never call Axios directly.
- **RBAC:** Role enums + permission helpers. 20 protected route targets across 2 groups. Role-filtered sidebar.
- **Generic data grid:** TanStack Table v8 reused across all 6 entity modules. Column search, filtering, visibility, pagination, print + multipage A4 PDF export.
- **12 type-safe forms** (React Hook Form + Zod). 2 dynamic list forms with `useFieldArray`.
- **Booking lifecycle:** Approve / decline / return transitions with per-status UI and React Query mutations.
- **Lighthouse:** Performance ~90 (improved from ~60 via code-splitting + TanStack Query caching).

**Common interview questions:**
- How do you handle API integration at scale? → Single Axios client + interceptors + 5 generic hooks
- How does the data grid work? → TanStack Table with search, filter, visibility, export. Reused across 6 modules.
- How do you improve performance? → React.lazy/Suspense code-splitting + TanStack Query caching

---

## HR / Personal Questions

### Traditional HR Questions

#### 1. Tell me about yourself
**Structure:** Present → Past → Future (1-2 minutes max)

> "I'm a frontend developer with 2+ years of experience shipping production React and TypeScript applications. At Kits Technology, I built the frontend for a car-rental marketplace — dashboards, RBAC, payment integrations — taking Lighthouse Performance from 60 to 90+. I've also built several full-stack projects including a multi-tenant e-commerce SaaS with Stripe and a task management platform with Supabase. I specialize in turning complex data into clean, performant UIs and I'm looking for a role where I can work on data-heavy applications and grow as an engineer."

**Tips:** Don't repeat your resume. Don't mention personal life. Focus on professional achievements relevant to the role.

#### 2. Why do you want to work for our company?
**Framework:** Their mission + your skills + growth

- Research their product, tech stack, recent news
- Connect: "I saw you're building [X] — I've built similar dashboards/APIs and I'd love to bring that experience"
- Show you understand their challenges

#### 3. What are your greatest strengths and weaknesses?
**Strengths (pick 2-3 with proof):**
- Ownership: "I take features from design through implementation and deployment — at Kits, I owned 38 REST endpoint integrations end-to-end"
- Problem-solving: "I enjoy finding the right abstraction — I built a 9-file CRUD template and reused it across 5 resources"
- Performance awareness: "I proactively optimized Lighthouse from 60 to 90+ through code-splitting and caching"

**Weaknesses (real + improving):**
- "I sometimes over-engineer solutions. I've learned to start with the simplest version and refactor only when needed."
- "I'm working on my system design skills — I've been studying scalability patterns and building projects to practice."

#### 4. Why are you looking for a change?
- Never badmouth current/previous employer
- Focus on growth: "I want to expand my horizons, work on more complex problems, and contribute to a larger engineering team"
- Connect to their company: "Your company's focus on [X] aligns with where I want to grow"

#### 5. Tell me about the gap in your resume
- Be honest and positive
- Frame it as growth: "I took time to upskill, build projects, and prepare for my next role"
- Mention what you learned during the gap

#### 6. How would you rate yourself on a scale of 1 to 10?
- Say 7-8: "I'm a strong 8 — I know I'm not perfect and there's always room to learn"
- Never say 10 (arrogant) or below 6 (lacking confidence)

#### 7. What is your biggest achievement so far?
**Use STAR format:**
> "At Kits Technology, we needed to improve dashboard performance. I analyzed the codebase and implemented React.lazy code-splitting on 20 route targets plus TanStack Query caching. Lighthouse Performance went from ~60 to 90+. The client was thrilled and it became the standard for all future features."

#### 8. Where do you see yourself in 5 years?
- Show commitment to the company: "Growing with the team, taking on more complex technical challenges"
- Avoid: "starting my own company" or "becoming a manager" (unless asked about leadership)
- Good: "Leading frontend architecture decisions and mentoring junior developers"

#### 9. Why should we hire you?
> "I don't just build UI — I think about the full picture: auth, payments, data flow, performance. I've shipped production apps end-to-end including a multi-tenant SaaS and a car-rental marketplace. I care about maintainability and I bring a track record of measurable improvements."

#### 10. How do you deal with criticism?
- "I welcome constructive feedback — it helps me grow. When I receive criticism, I ask clarifying questions, reflect on it, and adjust my approach."
- Show maturity: "If the feedback is negative, I focus on what I can learn and continue doing my best work."

---

### Behavioural HR Questions (STAR Format)

**STAR:** Situation → Task → Action → Result

#### 11. Tell me about a time when you were not satisfied with your performance
> "When I first started at Kits, I realized I was relying too heavily on senior developers for API integration decisions. I took ownership by studying the existing patterns, building a centralized Axios client with interceptors, and creating 5 generic request hooks. This became the standard for all API calls across the project."

#### 12. Tell me about a time when you were made to work under close supervision
> "On a tight deadline, a project manager wanted daily check-ins on my progress. At first it felt excessive, but I realized it was because the stakeholder needed visibility. I started proactively sharing progress updates and demoing completed features. The daily check-ins became weekly ones as trust grew."

#### 13. Can you tell me about a time where you were happy with your work?
> "When I delivered the StoreHub e-commerce platform, the client was amazed by the Lighthouse scores — 99 on admin, 97 on storefront. They had never seen performance like that from a previous vendor. It validated my approach of using React Server Components and careful code-splitting."

#### 14. Tell me about a time where you experienced difficulty at work while working on a project
> "On Taskly, I struggled with SSR hydration mismatches in the Zustand cart. The server-rendered HTML didn't match the client because of localStorage persistence. I solved it by implementing `skipHydration` and rehydrating on mount, with `isMounted` checks on dependent components. It taught me to think carefully about server/client boundaries in Next.js."

#### 15. Tell me about a time where you displayed leadership skills
> "On the Get2Cars project, I noticed every developer was making individual Axios calls with duplicated error handling. I proposed building a centralized API layer with interceptors. I built the prototype, got buy-in, and it became the pattern used across 13 dashboard tables and 38 endpoints."

#### 16. Was there any point in your career where you made any mistake?
> "Early on, I underestimated the complexity of a Stripe webhook integration. I didn't account for signature verification and idempotency. After a few edge cases failed in testing, I studied the Stripe docs thoroughly, implemented proper verification, and added retry logic. Now I always research external API requirements before starting integration."

#### 17. How did you handle disagreements with your manager?
> "I once disagreed with using Redux for server-state in a Next.js project. I presented a comparison: Redux for client state + TanStack Query for server state. I showed a working prototype demonstrating the benefits. My manager agreed and the project benefited from cleaner separation of concerns."

#### 18. Tell me how you will handle it if suddenly the priorities of a project were changed?
> "I'd first understand the business reason for the change, then quickly assess what's in-progress and what can be paused. I'd communicate with stakeholders about implications, reprioritize my tasks, and focus on the new priority. Flexibility is key — I trust that the company has good reasons for shifting direction."

---

### Opinion-Based HR Questions

#### 19. You win a million-dollar lottery. Would you still be working?
- "I'd be thrilled, but I wouldn't quit. I love building things and solving problems. Work gives me purpose and growth. I might take a vacation though!"
- Never say "yes I'd quit" — makes you seem disloyal

#### 20. What would you do if you were working under a bad boss?
- "I'd try to understand their perspective first. Maybe they're under pressure. I'd adapt my communication style and build trust through consistent delivery. If it persists, I'd escalate through proper channels."

#### 21. What do you think is an ideal work environment?
> "A team-oriented environment where people collaborate, share knowledge, and hold each other accountable. I value clear communication, technical mentorship, and a culture that ships iteratively rather than perfecting in isolation."

#### 22. What does motivation mean to you?
> "Solving problems and seeing users benefit from what I built. Every time I see a clean dashboard loading fast or a smooth payment flow, that's motivating. I also love learning new technologies — stagnation is my enemy."

#### 23. What is your dream company like?
> "A company that values engineering quality, invests in developer growth, and builds products that genuinely solve problems. Based on my research, your company does that — and that's why I'm excited about this opportunity."

#### 24. What would you prefer — being liked or being feared?
- "I'd prefer being respected. Fear doesn't build good teams. I want to be the person colleagues trust and reach out to for help."

#### 25. How long do you think you will be working for us?
> "As long as I'm growing, contributing, and feel valued. I'm looking for a long-term fit, not just a stepping stone."

#### 26. If you were reborn as an animal, what animal would you be?
- Pick something with relevant traits: "An owl — they're methodical, see the big picture, and work best when focused. That's how I approach problems."

#### 27. Will you lie for the company under any circumstances?
- "I believe in honesty. If a situation requires discretion rather than lying, I'd handle it professionally. But I wouldn't compromise my integrity."

---

### Brainteaser HR Questions

#### 28. Being perfect and delivering late vs being good and delivering on time?
- "Good and on time. There's always room for iteration. Late delivery damages trust more than imperfection damages quality."

#### 29. Judy's mother had 4 children... what was the name of the fourth child?
- Answer: **Judy** (it's in the question — think carefully before answering)

#### 30. How many times a day do clock hands overlap?
- Answer: **22 times** (not 24 — at 11:55 they don't overlap)

---

### Salary Related Questions

#### 31. What is your current salary?
- "I'm not comfortable sharing that, but I'd love to hear the range for this role."
- Research market rates on Glassdoor/LinkedIn

#### 32. What is your salary expectation?
- Give a range, not a fixed number
- Back it up: "Based on my 2+ years of production experience, the complexity of projects I've delivered, and market research, I'm looking for [range]"
- Always leave room for negotiation

#### 33. How much do you think you should be paid?
- "Based on my skills in React, Next.js, TypeScript, and the production applications I've shipped, I believe a competitive salary for this market would be [range]. I'm open to discussing based on the total compensation package."

---

### Questions to Ask the Interviewer
- "What does the development workflow look like here?"
- "How does the team handle technical debt?"
- "What's the biggest challenge the frontend team is facing right now?"
- "How do you approach code reviews?"
- "What's the tech stack and are there plans to evolve it?"
- "How do you measure success for this role?"

---

## Session Summaries

### Day 1 — JavaScript Fundamentals (2026-08-16)

**Quiz Score:** 7/10

**What was covered:**
- Closures and why `var` in loops creates shared references
- `this` inside arrow functions vs regular functions
- Hoisting behavior of `var` vs `let` (TDZ)
- `==` vs `===` and type coercion
- Event loop basics (sync → microtasks → macrotasks)
- Array methods (map, filter) and what they return
- Mutation vs copy (`b = arr` copies the reference, not the array)
- Destructuring basics
- `null` vs `undefined`
- Prototypes and the prototype chain
- `__proto__` vs `prototype`

**Questions and Answers:**

#### Q1: setTimeout with var in a loop
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```
**Answer:** `3 3 3`
**Why:** `var` has function scope, not block scope. All three callbacks share the same `i`. By the time they run, the loop has finished and `i === 3`.
**Interview tip:** If you see `var` inside a loop, think "shared variable". Use `let` to fix it — it creates a new `i` per iteration.

#### Q2: `this` in arrow function
```js
const obj = {
  name: "Ahmed",
  getName: () => this.name
};
obj.getName(); // undefined
```
**Answer:** `undefined`
**Why:** Arrow functions don't bind their own `this`. They inherit from the outer (global) scope, where `this.name` is undefined.
**Interview tip:** Arrow functions = no own `this`. Regular functions get `this` from how they're called.

#### Q3: Hoisting with var
```js
console.log(a);
var a = 5;
```
**Answer:** `undefined`
**Why:** JS splits it into: `var a;` (hoisted) → `console.log(a)` → `a = 5`. The declaration is hoisted but the assignment is not.
**Interview tip:** `var` → hoisted + initialized to `undefined`. `let`/`const` → hoisted but NOT initialized (TDZ → ReferenceError).

#### Q4: Array coercion
```js
[] == false // true
```
**Why:** Empty array → coerces to `""` → `""` becomes `0` → `false` becomes `0` → `0 == 0` is `true`.
**Interview tip:** `==` performs type coercion. `===` does not.

#### Q5: Event loop order
```js
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
console.log("D");
```
**Answer:** `A D C B`
**Why:** Sync code runs first (A, D), then microtasks (Promise → C), then macrotasks (setTimeout → B).
**Interview tip:** Order: sync → micro → macro.

#### Q6: Array methods
```js
[1, 2, 3, 4].filter(n => n % 2 === 0).map(n => n * 10)
```
**Answer:** `[20, 40]`
**Why:** filter → `[2, 4]`, then map → `[20, 40]`.
**Interview tip:** `.map()` always returns a new array of the same length.

#### Q7: Mutation vs copy
```js
const arr = [1,2,3];
const b = arr;
b.push(4);
arr.length; // 4
```
**Answer:** `4`
**Why:** `b = arr` copies the reference, not the array. Both point to the same array in memory.
**Interview tip:** To copy an array: `[...arr]` or `arr.slice()` or `structuredClone(arr)`.

#### Q8: Async — wait 1 second then return "done"
**Best answer:**
```js
const wait = () => new Promise(resolve => setTimeout(() => resolve("done"), 1000));
```
**Why:** Wrapping `setTimeout` in a `Promise` lets you use `await` and `.then()`.
**Interview tip:** Callback-based APIs → wrap in `Promise` → use `resolve`/`reject`.

#### Q9: Destructuring
```js
const {x, y} = point;
```
**Answer:** Correct.

#### Q10: Coercion
```js
"5" + 3   // "53" — string wins, concatenation
"5" - 3   // 2 — only math, JS coerces string to number
```
**Interview tip:** `+` = concatenation when either side is a string. All other operators coerce strings to numbers.

#### Q11: Prototypes — shared method on constructor
```js
function Animal(type) { this.type = type; }
Animal.prototype.speak = function() { return `${this.type} makes a sound`; };
const cat = new Animal("Cat");
const dog = new Animal("Dog");
cat.speak === dog.speak; // true — shared prototype method
cat.hasOwnProperty("speak"); // false — speak is on prototype, not cat
cat.hasOwnProperty("type"); // true — type is directly on cat
```
**Interview tip:** `hasOwnProperty` returns a boolean, checks only the object itself (not prototype chain). Use `in` operator to check the full chain.

#### Q12: Prototype chain lookup
```js
const obj = {};
obj.__proto__ = { a: 1 };
obj.__proto__.__proto__ = { b: 2 };
obj.a; // 1
obj.b; // 2
obj.c; // undefined — not found, chain ends at null
```
**Interview tip:** JS walks `obj.__proto__.__proto__...` until `null`. If not found → `undefined`, never an empty object.

---

### Day 2 — Event Loop + Promises + Async (2026-08-17)

**Quiz Score:** 6/14

**What was covered:**
- Event loop: sync → microtask → macrotask
- Promise.all short-circuits on first rejection
- Promise.allSettled waits for all, never rejects
- Promise executor runs synchronously — not deferred
- `resolve()` / `reject()` queue callbacks but don't stop execution
- `.then()` is always async — runs after call stack is empty
- `await` pauses async function, yields to microtask queue
- `.json()` is async — needs `await`
- `await` on resolved → unwraps value; on rejected → throws

**Questions and Answers:**

#### Q1: Promise executor order
```js
const p = new Promise((resolve) => {
  console.log("A");
  resolve("B");
  console.log("C");
});
p.then(v => console.log(v));
console.log("D");
```
**Answer:** `A C D B`
**Why:** Executor is sync (A, C). resolve("B") queues the .then callback. D runs sync. Then "B" runs as microtask.
**Interview tip:** The Promise executor is just a regular function call — runs immediately, not deferred.

#### Q2: Promise.all short-circuits
```js
Promise.all([Promise.resolve(1), Promise.reject(2), Promise.resolve(3)])
  .then(r => console.log(r))
  .catch(e => console.log(e));
```
**Answer:** `2`
**Why:** Promise.all stops at first rejection. .then never runs.
**Interview tip:** Promise.all = all-or-nothing. Use Promise.allSettled if you want all results.

#### Q3: Missing await on .json()
```js
const data = res.json(); // BUG: returns Promise, not data
```
**Answer:** Need `await res.json()` — .json() is async (reads a stream).
**Interview tip:** fetch() gives you a Response object. You need await to parse the body.

#### Q4: async function with resolved Promise
```js
async function foo() {
  console.log("A");
  const p = Promise.resolve("B");
  const result = await p;
  console.log(result);
}
console.log("1");
foo();
console.log("2");
```
**Answer:** `1 A 2 B`
**Why:** await pauses foo(), sync code (2) runs first, then microtask (B).
**Interview tip:** await pauses the function — it doesn't run the rest immediately.

#### Q5: return vs return await
```js
// Version A
return res.json();      // returns Promise<JSON>

// Version B
return await res.json(); // returns Promise<JSON> but json error is caught by this try/catch
```
**Answer:** Same return value. Difference is error handling scope.
**Interview tip:** In most cases, behavior is identical. Use return await if you need the catch block to handle .json() errors.

#### Q6: await with rejected Promise
```js
async function foo() {
  try {
    const x = await Promise.reject("error");
  } catch (e) {
    console.log("caught:", e);
  }
  console.log("done");
}
```
**Answer:** `caught: error` then `done`
**Why:** catch doesn't stop execution — function continues after try/catch.
**Interview tip:** try/catch in async functions works like synchronous try/catch — code continues after catch block.

**Key rules:**
- Promise executor is sync — runs immediately
- `resolve()` queues .then callback, doesn't stop executor
- `.then()` runs after call stack empties (microtask)
- `await` = same as resolve — puts rest of function on microtask queue
- `Promise.all` = all-or-nothing. `Promise.allSettled` = always resolves

---

**Gaps to review before next session:**
- Promise executor runs synchronously (not deferred)
- resolve()/reject() don't stop execution
- `await` pauses function and yields to microtask queue
- Promise.all short-circuits, allSettled doesn't
- Block vs function scope (`var` vs `let` vs `const`)

---

### HR Practice Session — Q1-Q3 (2026-08-17)

**Q1: Tell me about yourself**

**Strong answer:**
> "I'm a frontend developer with 2+ years of experience shipping production React and TypeScript applications. At Kits Technology, I built the frontend for a car-rental marketplace — integrating 38 REST endpoints, implementing RBAC across 20 routes, and improving dashboard performance from 60 to 90+ Lighthouse score through code-splitting and caching. I've also built several full-stack projects including a multi-tenant e-commerce SaaS with Stripe payments and a task management platform with an optimistic Kanban board. I specialize in turning complex data into clean, performant interfaces and I'm looking for a role where I can work on data-heavy applications and grow as an engineer."

**Structure:** Present (what you do) → Past (key achievements) → Future (what you want)

**Q2: Strengths & Weaknesses**

**Strengths — needs proof:**
- "I love learning" → too vague
- Better: "One of my greatest strengths is ownership. At Kits, I built a centralized API layer with interceptors that became the standard for 38 endpoints. I also proactively optimized Lighthouse from 60 to 90+."

**Weakness — perfectionism is a cliché:**
- Better: "I sometimes spend too much time optimizing before shipping. On a data table component, I initially over-engineered the filtering logic. I've learned to ship the simplest version first and refactor only when needed — now I timebox optimization to after the feature works."

**Q3: Why should we hire you?**

**Too generic:** "I care about the project" — every candidate says this.

**Better:**
> "I don't just build UI — I think about the full picture. At Kits, I integrated 38 endpoints, implemented RBAC, and improved performance by 50%. On personal projects, I've built Stripe payments, optimistic updates, and multi-tenant isolation. I bring production experience with complex features, and I care about maintainability."

---

### Day 3 — ES6+ (Technical) + HR (2026-08-18)

#### Session A — Technical: ES6+ Methods
*(to be completed — see plan day 3)*

#### Session B — HR: Q4-Q6 (pending)

**Q4:** Why are you looking for a change?
**Q5:** Tell me about a time where you experienced difficulty at work.
**Q6:** Where do you see yourself in 5 years?

---

*Last updated: 2026-08-18*
