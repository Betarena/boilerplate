---

<details open="open">
<summary>📑 Table of Contents</summary>

- [🟧 Svelte/+Kit](#-sveltekit)
  - [🚏 Creating new routes](#-creating-new-routes)
  - [🔄 Creating new load(s)](#-creating-new-loads)
  - [⭐️ Creating new components](#️-creating-new-components)
    - [🎡 Complex state widgets](#-complex-state-widgets)
  - [🚫 Forbidden Code Blocks](#-forbidden-code-blocks)
    - [🔥 Derived Reactivity Statements from Svelte Stores of type `object`.](#-derived-reactivity-statements-from-svelte-stores-of-type-object)
      - [🟥 Disallowed](#-disallowed)
      - [🟩 Exepected](#-exepected)
    - [📉 Nested Component Property Drilling](#-nested-component-property-drilling)
      - [🟥 Disallowed](#-disallowed-1)
      - [🟩 Exepected](#-exepected-1)
  - [💠 Styling Preferrence](#-styling-preferrence)
    - [🎨 Declare `:global()` using cleaner structure](#-declare-global-using-cleaner-structure)
      - [🟥 Disallowed](#-disallowed-2)
      - [🟩 Exepected](#-exepected-2)
</details>

---

### 🟧 Svelte/+Kit

#### ───────────────────────────────────────────

#### 🚏 Creating new routes

> [!CAUTION]
> Any endpoint created/updated should be documented in an already existing [`openapi.yaml`](https://swagger.io/specification/), located in the project root `(./)` or elsewhere.

When creating new `.ts` routes (a.k.a endpoints), please make sure to follow the following folder structrue:

```markdown
src/
├── lib/
    └── sveltekit/
        └── endpoint/
            ├── <data|widget>.<name>.ts
            ├── <data|widget>.<name>.ts
            └── <data|widget>.<name>.ts
└── routes/
    └── api/
        └── [...path]/
            └── +server.ts
```

All routes should be placed using conditions into `+server.ts`, with their respective logic offloaded into sub-modules.

⭐️ Examples of this can be found:
- [1a] - [src/lib/sveltekit/endpoint/*](https://github.com/Betarena/betarena_about/tree/feature/public-presale/draft/1-2-3/src/lib/sveltekit/endpoint)
- [1b] - [src/routes/api/data/\[...path\]/+server.ts](https://github.com/Betarena/betarena_about/blob/feature/public-presale/draft/1-2-3/src/routes/api/data/%5B...path%5D/%2Bserver.ts)

#### ───────────────────────────────────────────

#### 🔄 Creating new load(s)

In a similar fashion to [🚏 Creating new routes](#-creating-new-routes), load functions should be structured as follows:

```markdown
src/
└── lib/
    └── sveltekit/
        └── load/
            ├── <page-name>.ts
            └── <page-name>.ts
```

These `<page-name>.ts` sub-modules should be consumed only by their `+page(.server).ts` counterpart(s) located in the `routes/*` directory.

⭐️ Examples of this can be found:
- [1a] - [scores/src/routes/(scores)/u/\[view\]/\[lang=lang\]
/+page.server.ts](https://github.com/Betarena/scores/blob/main/src/routes/(scores)/u/%5Bview%5D/%5Blang%3Dlang%5D/%2Bpage.server.ts)
- [1a] - [scores/src/lib/sveltekit/load
/load.lang.ts](https://github.com/Betarena/scores/blob/main/src/lib/sveltekit/load/load.lang.ts)

#### ───────────────────────────────────────────

#### ⭐️ Creating new components

When creating new `.svelte` components, please make sure to follow the following folder structure for respective widget.

```markdown
lib/
└── components/
    └── <-INSERT-TARGET-SECTION/PAGE/GROUP-COMPONENT-WILL-BELONG-TO->/
        └── <-INSERT-COMPONENT-NAME->/
                ╭───
                │: 📝 Contains respective new widget/component (1) assets, (2) component code, (3) preloaders, etc.
                ╰───
            ├── .assets/
                ╭───
                │: 📝 Contains respective assets used by THIS widget/component.
                ╰───
                ├── *.png
                └── *.svg
            ├── loaders/
                ╭───
                │: 📝 Contains respective loaders.
                ╰───
            ├── New-Widget-Widget.svelte
                ╭───
                │: 📝 Is the MAIN entry point to the widget that is being created, think of it as the *handler*
                │:    for the widget, containing "data" getter for the widget, and showing loaders.
                ╰───
            ├── New-Widget-Main.svelte
                ╭───
                │: 📝 Is the MAIN widget layout, design and logic, after the parent [...]-Widget.svelte has loaded all necessary
                │:    data and deemed it OK to show the widget.
                │:    THIS contains the overall MAIN widget UI and LOGIC. As well as, necessary child components.
                ╰───
            ├── New-Widget-Loader.svelte
                ╭───
                │: 📝 Is the MAIN widget loader layout, used for showing the widget outline and it's preloading-state. Used in
                │:    conjuction with the .svelte files in the laoders/ folder, containing .svg elements within.
                │:    Not always used, because some widgets do not have a pre-loader animation, in which case, this widget/component can be ignored.
                ╰───
```

⭐️ Examples of this can be found:
- [scores/src/lib/components/page/profile/investor](https://github.com/Betarena/scores/tree/main/src/lib/components/page/profile/investor)

##### 🎡 Complex state widgets

If a new `widget/component` is too complicated, such as it contains many `.svelte` files and/or has a complex state system, **ALWAYS** create a internal `_store.ts` file for the respective `widget/component`.

⭐️ Examples of this can be found:
- [about/src/lib/ui/components/presale/presale-box](https://github.com/Betarena/betarena_about/blob/feature/public-presale/draft/1-2/src/lib/ui/components/presale/presale-box) ➤ `_store.ts`.
- [scores/src/lib/components/_main_/auth](https://github.com/Betarena/scores/tree/main/src/lib/components/_main_/auth) ➤ `_store.ts`.
- [scores/src/lib/components/page/profile/investor](https://github.com/Betarena/scores/tree/main/src/lib/components/page/profile/investor) ➤ `_store.ts`.

#### ───────────────────────────────────────────

#### 🚫 Forbidden Code Blocks

##### ───────────────────────────────────────────

##### 🔥 Derived Reactivity Statements from Svelte Stores of type `object`.

###### 🟥 Disallowed

```svelte
<script lang='ts'>
  $: value = ($someStore as object).someProperty;
</script>
```

###### 🟩 Exepected

```svelte
<script lang='ts'>
  $: ({ someProperty } = $someStore);
</script>
```

> [!NOTE]
> **REASON:**
> 
> Because it will re-trigger `value = [..]` each time the `$someStore` changes in any way.
> 
> Including due to the change of some other (a.k.a `$someStore.property`) value, which can lead to the dreaded `Reactivity Infinity Loop`.

##### ───────────────────────────────────────────

##### 📉 Nested Component Property Drilling

###### 🟥 Disallowed

```svelte
// 📝 Component-A.svelte

<script lang='ts'>
  import ComponentB from '.Component-B.svelte';

  export let data: unknown;
</script>

<ComponentB {data} />
```

```svelte
// 📝 Component-B.svelte

<script lang='ts'>
  import ComponentC from '.Component-C.svelte';

  export let data: unknown;
</script>

<ComponentC {data} />
```

###### 🟩 Exepected

```svelte
// 📝 Component-B.svelte

<script lang='ts'>
  import ComponentC from '.Component-C.svelte';

  $: ({ data } = $someLocalStore);
</script>

<ComponentC />
```

```svelte
// 📝 Component-C.svelte

<script lang='ts'>
  $: ({ data } = $someLocalStore);
</script>
```

> [!NOTE]
> **REASON:**
>
> (1) Leads to bad coding structure, which directly affects respective file structure and (2) is difficult to track data dependency when making changes or maintaining code.
>
> **ALWAYS, ALWAYS** extract from an: (1) independent store or (2) an alternative data access module for such components, that would otherwise depend on each other to pass along data.

#### ───────────────────────────────────────────

#### 💠 Styling Preferrence

##### ───────────────────────────────────────────

##### 🎨 Declare `:global()` using cleaner structure

###### 🟥 Disallowed

```scss
:global(html) {
  overflow-x: hidden;
  overflow-y: overlay;
}

:global(body) {
  :global(figure) {
     margin: 0;
   }
}

:global(img) {
  max-width: 100%;
  display: block;
}

:global(button) {
  cursor: pointer;
}
```

###### 🟩 Exepected

```scss
:global
{
  html
  {
    overflow-x: hidden;
    overflow-y: overlay;
  }

  body
  {
    figure
    {
      margin: 0;
    }
  }
  
  img
  {
    max-width: 100%;
    display: block;
  }
  
  button
  {
    cursor: pointer;
  }
}
```

> [!NOTE]
> **REASON:**
>
> Style simplification for easier readability and identification of conflicting `styles` when doing fixes.
>
> Make sure that `:global { .. }` is used instead of `:global(..) { .. }`.
