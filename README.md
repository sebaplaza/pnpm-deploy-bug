# pnpm-deploy-bug

pnpm deploy --prod problem

In a monorepo, the `pnpm --filter deploy` command is copying **ALL** production packages, even from others projects.

## Reproduce
```sh
corepack enable # to use the right version of pnpm
rm -rf my-app-dist && pnpm --filter "my-app" --prod deploy my-app-dist
```

The result is the next one

```sh
.
├── apps
│   ├── my-app
│   │   └── package.json
│   └── other-app
│       └── package.json
├── my-app-dist
│   ├── node_modules
│   │   ├── date-fns -> .pnpm/date-fns@2.29.3/node_modules/date-fns
│   │   └── webpack -> .pnpm/webpack@5.79.0/node_modules/webpack
│   └── package.json
├── node_modules
├── package.json
├── pnpm-workspace.yaml
└── README.md
```

We should **NOT** have `webpack` inside our `my-app-dist/node_modules` folder, since is it belongs to `other-app` project.

and we can also find `prettier` in `my-app-dist/node_modules/.pnpm/prettier@2.8.7`, that belongs to root monorepo package.json
