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

An example can be shown [here](https://github.com/Betarena/betarena_about/blob/feature/public-presale/draft/1-2/src/lib/ui/components/presale/presale-box/_store.ts).

#### ───────────────────────────────────────────

#### 🚫 Forbidden Code Blocks

##### 🔥 Derived Reactivity Statements from Svelte Stores of type `object`.

###### 🟥 Disallowed

```typescript
$: value = ($someStore as object).someProperty;
```

REASON: Because it will re-trigger `value = [..]` each time the `$someStore` changes in any way, including due to the change of other `property` values.

###### 🟩 Exepected

```typescript
$: ({ someProperty } = $someStore);
```
