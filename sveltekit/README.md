#### ⭐️ Creating new `.svelte` components (a.k.a widgets) should be structured as follows:

```markdown
/lib/
  ─────────────────────────────────────────────────────────────────────────────────────────────
  ↳ components/
    ─────────────────────────────────────────────────────────────────────────────────────────────
    ↳ <-INSERT-target-section-belongs-to->/
      ─────────────────────────────────────────────────────────────────────────────────────────────
      ↳ <-INSERT-widget->/
          | 📣 Contains respective new widget/component (1) assets, (2) component code, (3) preloaders, etc.
        ─────────────────────────────────────────────────────────────────────────────────────────────
        ↳ .assets/
            | 📝 Contains respective assets used by THIS widget/component.
          ↳ *.png
          ↳ *.svg
        ─────────────────────────────────────────────────────────────────────────────────────────────
        ↳ loaders/
            | 📝 Contains respective loaders.
        ─────────────────────────────────────────────────────────────────────────────────────────────
        ↳ New-Widget-Widget.svelte
            | 📝 Is the MAIN entry point to the widget that is being created, think of it as the *handler*
            |    for the widget, containing "data" getter for the widget, and showing loaders.
            | 📌 Check 'src/lib/components/page/profile/investor/Widget-Investor.svelte' for example.
        ─────────────────────────────────────────────────────────────────────────────────────────────
        ↳ New-Widget-Main.svelte
            | 📝 Is the MAIN widget layout, design and logic, after the parent [...]-Widget.svelte has loaded all necessary
            |    data and deemed it OK to show the widget.
            |    THIS contains the overall MAIN widget UI and LOGIC. As well as, necessary child components.
        ─────────────────────────────────────────────────────────────────────────────────────────────
        ↳ New-Widget-Loader.svelte
            | 📝 Is the MAIN widget loader layout, used for showing the widget outline and it's preloading-state. Used in
            |    conjuction with the .svelte files in the laoders/ folder, containing .svg elements within.
            |    Not always used, because some widgets do not have a pre-loader animation, in which case, this widget/component can be ignored.
```

##### 🎡 Complex state widgets

If a new `widget/component` is too complicated, such as it contains many `.svelte` files and/or has a complex state system, **ALWAYS** create a internal `_store.ts` file for the respective `widget/component`.

Examples of this can be found:
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
> REASON: Because it will re-trigger `value = [..]` each time the `$someStore` changes in any way, including due to the change of other `property` values.

##### ───────────────────────────────────────────

##### 💠 Nested Component Property Drilling

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

<ComponentC {data} />
```

```svelte
// 📝 Component-C.svelte

<script lang='ts'>
  $: ({ data } = $someLocalStore);
</script>
```

> [!NOTE]
> REASON: Leads to bad coding structure and difficult to track data dependency. **ALWAYS** draw from a (1) store or (2) independent data access for such components that would otherwise depend on each other to pass along data.
