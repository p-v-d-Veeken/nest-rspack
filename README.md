# NestRspack

The purpose of this repo is to demonstrate an issue with NestJS and RSPack where after running `nx build`, the resulting JS bundle will not start without node_modules being present, which defeats the entire purpose of using a bundler in the first place.

## Steps to reproduce

1. Clone this repo
2. Run `npm install`
3. Run `nx build demo`
4. Run `node dist/apps/demo/main.js` this will output:
```
[Nest] 14258  - 10/13/2023, 2:34:34 PM     LOG [NestFactory] Starting Nest application...
[Nest] 14258  - 10/13/2023, 2:34:34 PM     LOG [InstanceLoader] AppModule dependencies initialized +14ms
[Nest] 14258  - 10/13/2023, 2:34:34 PM     LOG [RoutesResolver] AppController {/api}: +20ms
[Nest] 14258  - 10/13/2023, 2:34:34 PM     LOG [RouterExplorer] Mapped {/api, GET} route +3ms
[Nest] 14258  - 10/13/2023, 2:34:34 PM     LOG [NestApplication] Nest application successfully started +3ms
[Nest] 14258  - 10/13/2023, 2:34:34 PM     LOG 🚀 Application is running on: http://localhost:3000/api
```
5. Run `mv node_modules node_modules.tmp`
6. Run `node dist/apps/demo/main.js` again, this time it will output:
```
/Users/user/code/nest-rspack/dist/apps/demo/main.js:721
            throw e;
            ^

Error: Cannot find module '@nestjs/common'
Require stack:
- /Users/user/code/nest-rspack/dist/apps/demo/main.js
    at Module._resolveFilename (node:internal/modules/cjs/loader:1077:15)
    at Module._load (node:internal/modules/cjs/loader:922:27)
    at Module.require (node:internal/modules/cjs/loader:1143:19)
    at require (node:internal/modules/cjs/helpers:121:18)
    at @nestjs/common (/Users/user/code/nest-rspack/dist/apps/demo/main.js:4:18)
    at __webpack_require__ (/Users/user/code/nest-rspack/dist/apps/demo/main.js:718:33)
    at fn (/Users/user/code/nest-rspack/dist/apps/demo/main.js:798:10)
    at ./apps/demo/src/main.ts (/Users/user/code/nest-rspack/dist/apps/demo/main.js:674:70)
    at __webpack_require__ (/Users/user/code/nest-rspack/dist/apps/demo/main.js:718:33)
    at /Users/user/code/nest-rspack/dist/apps/demo/main.js:1647:27 {
  code: 'MODULE_NOT_FOUND',
  requireStack: [
    '/Users/user/code/nest-rspack/dist/apps/demo/main.js'
  ]
}
```
