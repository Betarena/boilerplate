# 🔀 Version Control

This markdown goes over the workflow regarding version control employed at Betarena.

> [!IMPORTANT]
> The structure should be enforced across all Betarena projects for keeping consistency and the ability to quickly generate `CAHNGELOGs`, for example: [scores-lib](https://github.com/Betarena/scores-lib/blob/main/CHANGELOG.md).

## 📝 Commits

This project follows the use of [**`conventional commits`**](https://www.conventionalcommits.org/en/v1.0.0/), enforced by [**`husky`**](https://github.com/typicode/husky).

**📌 Commit Example**

A typical commit would look like just any other **conventional commit**, with only changes to the `commit description`, presenting it as such:

```md
refactor(example): showcase

[1] ────
<insert-commit-notable-change-description-here>
[2] ────
<insert-another-commit-notable-change-description-here>
[3] ────
<etc.>

#[optional footer(s)]
```

**⭐️ Example**

<img 
  width="686" 
  alt="image" 
  src="https://github.com/user-attachments/assets/76b2224b-65e8-41d1-ae78-45bd3d660322"
/>

↳ https://github.com/Betarena/scores-lib/pull/391

### 💠 Recommendations

If you are using `VsCode`, make use of the `adam-bender.commit-message-editor` extension that would ease up commit message write up.

## 🔀 Branches

The used branching strategy at **Betarena** is [`Git Flow`](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow), enforced by [**`husky`**](https://github.com/typicode/husky).

**📌 Branch Example**

A typical branch name would take the following `regex` form:

`^(master|main|develop|dev){1}$|^(feature|fix|hotfix|release|refactor|chore).+$`

for example:

```markdown
🟩 feature/<some-new-feature-name>/draft/0
🟩 feature/<some-other-feature-name>/draft/1
🟩 fix/<some-fix-name>/draft/0
🟩 refactor/<some-refactor-name>/draft/0
```
