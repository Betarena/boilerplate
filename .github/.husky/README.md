# 🐶 Husky

If a new project is created, and/or an existing project is not making use of `husky` follow the following steps:

## Update `package.json`

Make sure that the contents of the `./package.json` are as follows:

```json
{
  [..]
  "devDependencies": 
  {
    "@commitlint/cli": "17.8.1",
    "@commitlint/config-conventional": "17.8.1",
    "conventional-changelog-cli": "4.1.0",
    "cz-conventional-changelog": "3.3.0",
    "husky": "8.0.3",
    "is-ci": "3.0.1",
    "validate-branch-name": "1.3.0"
  },
  "config":
  {
    "commitizen":
    {
      "path": "./node_modules/cz-conventional-changelog"
    }
  },
  "commitlint":
  {
    "extends":
    [
      "@commitlint/config-conventional"
    ],
    "rules":
    {
      "body-max-line-length":
      [
        0,
        "always",
        1000
      ]
    }
  },
  "validate-branch-name":
  {
    "pattern": "^(master|main|develop|dev){1}$|^(feature|fix|hotfix|release|refactor|chore){1}\\/.+\\/draft\\/\\d+$",
    "errorMsg": "❌ Uh-oh! Please check package.json > validate-branch-name for allowed branch names!"
  }
  [..]
}
```
