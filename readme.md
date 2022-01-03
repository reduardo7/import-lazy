# import-lazy

> Import a module lazily


## Install

```
$ npm install import-lazy
```


## Usage

```js
// Pass in `require` or a custom import function
const importLazy = require('import-lazy')(require);
const _ = importLazy('lodash');

// Instead of referring to its exported properties directly…
_.isNumber(2);

// …it's cached on consecutive calls
_.isNumber('unicorn');

// Works out of the box for functions and regular properties
const stuff = importLazy('./math-lib');
console.log(stuff.sum(1, 2)); // => 3
console.log(stuff.PHI); // => 1.618033
```


## Note

### Destructuring will cause it to fetch eagerly

While you may be tempted to do leverage destructuring, like this:

```js
const {isNumber, isString} = importLazy('lodash');
```

Note that this will cause immediate property access, negating the lazy loading, and is equivalent to:

```js
import {isNumber, isString} from 'lodash';
```

### Usage with bundlers

If you're using a bundler, like Webpack, you'll have to [import your modules like this](https://github.com/webpack/webpack/issues/9155) in order to have them properly bundled:

```js
const importLazy = require('import-lazy');

const _ = importLazy(() => require('lodash'))();
```

### Usage with TypeScipt

At _TypeScript_, you should use following:

```typescript
import _importLazy from 'import-lazy';

const importLazy = _importLazy(require);
const _ = importLazy('lodash');
```

Or, with types:

```typescript
import _importLazy from 'import-lazy';
import type TLodash from 'lodash'; // Important! Note the import as `type`

const importLazy = _importLazy(require);
const _ = importLazy('lodash') as typeof TLodash;
```

#### TypeScript with Generics

With an auxiliar function, to reduce the code and have the _Types_ working:

- `src/utils/lazy-import.ts`
  ```typescript
  import _importLazy from 'import-lazy';

  const importLazy: any = (
    importer: typeof require,
    module: string
  ) =>  importLazy(importer)(moduleId);

  export default lazyImport as <T extends any>(
    importer: typeof require,
    module: string
  ) => T;
  ```
- `src/app/script.ts`. Usage:
  ```typescript
  import lazyImport from '../utils/lazy-import';

  import type TLodash from 'lodash'; // Important! Note the import as `type`
  import type TFoo from './foo'; // Important! Note the import as `type`
  import type * as TBar from './bar'; // No `default` defined. Important! Note the import as `type`
  import type * as TTypes from './types'; // This is a type. Important! Note the import as `type`

  const _ = importLazy<typeof TLodash>(require, 'lodash');
  const foo = importLazy<typeof TFoo>(require, './foo');
  const bar = importLazy<typeof TBar>(require, './bar');
  const types = importLazy<TTypes>(require, './types'); // Types not requires `typeof`
  ```

> Note: The `require` should come from some source where the Import should be done.

## Related

- [resolve-from](https://github.com/sindresorhus/resolve-from) - Resolve the path of a module from a given path
- [import-from](https://github.com/sindresorhus/import-from) - Import a module from a given path
- [resolve-pkg](https://github.com/sindresorhus/resolve-pkg) - Resolve the path of a package regardless of it having an entry point
- [lazy-value](https://github.com/sindresorhus/lazy-value) - Create a lazily evaluated value
- [define-lazy-prop](https://github.com/sindresorhus/define-lazy-prop) - Define a lazily evaluated property on an object


---

<div align="center">
	<b>
		<a href="https://tidelift.com/subscription/pkg/npm-import-lazy?utm_source=npm-import-lazy&utm_medium=referral&utm_campaign=readme">Get professional support for this package with a Tidelift subscription</a>
	</b>
	<br>
	<sub>
		Tidelift helps make open source sustainable for maintainers while giving companies<br>assurances about security, maintenance, and licensing for their dependencies.
	</sub>
</div>
