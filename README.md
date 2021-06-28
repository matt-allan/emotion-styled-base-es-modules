# @emotion/styled native ES modules issue

Setup:

```
yarn install
yarn build
```

Reproduction steps:

```
node lib/index.js
```

Expected output:

```
<ref *1> [Function (anonymous)] {
  displayName: 'Content',
  defaultProps: undefined,
  __emotion_real: [Circular *1],
  __emotion_base: 'div',
  __emotion_styles: [
    'label:Content;',
    {
      name: '1d3w5wq',
      styles: 'width:100%',
      map: '/*# sourceMappingURL=data:application/json;charset=utf-8;base64,eyJ2ZXJzaW9uIjozLCJzb3VyY2VzIjpbIi4uL3NyYy9pbmRleC5qcyJdLCJuYW1lcyI6W10sIm1hcHBpbmdzIjoiQUFFMEIiLCJmaWxlIjoiLi4vc3JjL2luZGV4LmpzIiwic291cmNlc0NvbnRlbnQiOlsiaW1wb3J0IHN0eWxlZCBmcm9tICdAZW1vdGlvbi9zdHlsZWQnO1xuXG5jb25zdCBDb250ZW50ID0gc3R5bGVkLmRpdmBcbiAgd2lkdGg6IDEwMCU7XG5gO1xuXG5jb25zb2xlLmxvZyhDb250ZW50KTsiXX0= */',
      toString: [Function: _EMOTION_STRINGIFIED_CSS_ERROR__]
    }
  ],
  __emotion_forwardProp: undefined,
  withComponent: [Function (anonymous)]
}
```

Actual output:

```
node:internal/process/esm_loader:74
    internalBinding('errors').triggerUncaughtException(
                              ^

Error [ERR_UNSUPPORTED_DIR_IMPORT]: Directory import '/Users/matt/code/emotion-styled-base-es-modules/node_modules/@emotion/styled/base' is not supported resolving ES modules imported from /Users/matt/code/emotion-styled-base-es-modules/lib/index.js
Did you mean to import @emotion/styled/base/dist/emotion-styled-base.cjs.js?
    at new NodeError (node:internal/errors:363:5)
    at finalizeResolution (node:internal/modules/esm/resolve:303:17)
    at moduleResolve (node:internal/modules/esm/resolve:742:10)
    at Loader.defaultResolve [as _resolve] (node:internal/modules/esm/resolve:853:11)
    at Loader.resolve (node:internal/modules/esm/loader:89:40)
    at Loader.getModuleJob (node:internal/modules/esm/loader:242:28)
    at ModuleWrap.<anonymous> (node:internal/modules/esm/module_job:73:40)
    at link (node:internal/modules/esm/module_job:72:36) {
  code: 'ERR_UNSUPPORTED_DIR_IMPORT',
  url: 'file:///Users/matt/code/emotion-styled-base-es-modules/node_modules/@emotion/styled/base'
}
```

You can patch it by manually rewriting the import to something Node can load:

```diff
- import _styled from "@emotion/styled/base";
+ import styledBase from "@emotion/styled/base/dist/emotion-styled-base.cjs.js";
+ const { default: _styled } = styledBase;
```
