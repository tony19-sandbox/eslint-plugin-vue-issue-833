> Demo for linter bug in TypeScript projects, where lines are incorrectly untouched when configuring `vue/script-indent` to ignore nested objects/arrays ([`vuejs/eslint-plugin-vue#833`](https://github.com/vuejs/eslint-plugin-vue/issues/833))

## Steps to reproduce

 1. Clone this repo, and run the linter:

        git clone https://github.com/tony19-sandbox/eslint-plugin-vue-issue-833.git
        npm run lint

 2. Observe `foo()` in `App.vue` is formatted as:

          methods: {
            foo() {
              const x = {
        a: [],
        b: 1,             // <-- FIXME: should be indented
        c: {},
        d: 2              // <-- FIXME: should be indented
              }
              console.log(x)
            }
          }

## Original project setup

 1. Generate a Vue CLI project with `vue create`, and pick the default setup at the prompt.

 2. In `package.json > eslintConfig > rules`, add the following rule to ignore nested arrays/objects, excluding top level from the exported object in `.vue`:

        "vue/script-indent": [
          "error",
          2,
          {
            "ignores": [
              "[value.type='ObjectExpression']:not(:matches(ExportDefaultDeclaration, [left.property.name='exports']) > * > [value.type='ObjectExpression'])",
              "[value.type='ArrayExpression']"
            ]
          }
        ]

 3. In `App.vue`, add the following unindented code:

        methods: {
        foo() {
        const x = {
        a: [],
        b: 1,
        c: {},
        d: 2
        }
        console.log(x)
        }
        }
