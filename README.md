# Angular Projects Style Guide

* Use appropriate prefix for new projects instead of `app`.
* Use [prettier](https://prettier.io/), [husky](https://github.com/typicode/husky) and [lint-staged](https://github.com/okonet/lint-staged) to support consistent code style.
* Clear your subscriptions on `NgDestroy` using [Unsubscribe](https://gist.github.com/phassttt/4214b29983982e7cce8b672ad4121e7c) class.
* Do not create `SharedModule`. All the common components that is not part of a specific routing page should be placed inside `src/lib` directory. Create separate modules for every semantically similar groups of components. For example, create `lib/forms/forms.module.ts` for forms related components.
* Use `Type | undefined` for `@Input()` fields to represent missing input value.

    ```typescript
        export class SomeComponent {
            @Input()
            posts: Posts | undefined;
        }
    ```

* Use `$` at the end of class field name for Observables.

    ```typescript
        export class SomeClass {
          private posts$: Observable<Posts>;
        }
    ```

## How to configure Prettier and TSLint

1. Install `prettier`, `husky`, `lint-staged` and `tslint-config-prettier` using command:

    ```bash
        # if you are using npm:
        npm install --save-dev prettier husky lint-staged tslint-config-prettier
        # if you are using yarn:
        yarn add --dev prettier husky lint-staged tslint-config-prettier
    ```

2. Create `prettier` configuration file in the root directory of your project. Call the configuration file `prettier.config.js`. Use [prettier documentation](https://prettier.io/docs/en/options.html) to set options.



    **Prettier configuration I use can be found [here](https://gist.github.com/phassttt/cb99f5fc91ef049c7747527e26f8c634). It works well with Angular projects.**

3. Add this lines at the end of your `package.json` file:

    ```json
      "husky": {
        "hooks": {
          "pre-commit": "lint-staged"
        }
      },
      "lint-staged": {
          "*.{js,json,ts,css,scss,html}": [
          "prettier --write",
          "git add"
        ]
      }
    ```

4. Then, extend your tslint.json, and make sure `tslint-config-prettier` is at the end:

    ```json
        "extends": [
            "tslint:latest",
            "tslint-config-prettier"
        ]
    ```

## How to use `Unsubscribe` class

```typescript

import { Unsubscribe } from "...";

@Component({
    // ...
})
export class SomeComponent extends Unsubscribe {
  fetchPosts() {
      return this.http.get('http://my-project.com/posts').pipe(
          // clears this subscription on NgDestroy life cycle hook
          this.takeUntilDestroy()
      );
  }
}
```

