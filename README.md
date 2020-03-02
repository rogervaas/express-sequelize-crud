# express-sequelize-crud

Simply expose resource CRUD (Create Read Update Delete) routes for Express & Sequelize. Compatible with [React Admin Simple Rest Data Provider](https://github.com/marmelab/react-admin/tree/master/packages/ra-data-simple-rest)

[![codecov](https://codecov.io/gh/lalalilo/express-sequelize-crud/branch/master/graph/badge.svg)](https://codecov.io/gh/lalalilo/express-sequelize-crud) [![CircleCI](https://circleci.com/gh/lalalilo/express-sequelize-crud.svg?style=svg)](https://circleci.com/gh/lalalilo/express-sequelize-crud)

## Install

```
yarn add express-sequelize-crud
```

## Usage

### Simple use case

```ts
import express from 'express'
import crud from 'express-sequelize-crud'
import { User } from './models'

const app = new express()
app.use(crud('/admin/users', User))
```

### Limit actions

```ts
import express from "express";
import crud, { Action } from "express-sequelize-crud";
import { User } from "./models";

const app = new express();
app.use(
  crud("/admin/users", User, {
    actions: [Action.GET_LIST, Action.GET_ONE]

    // or list disabled actions (this option override the action option)
    disabledActions: [Action.DELETE]
  })
);
```

### Hooks

```ts
import express from 'express'
import crud, { Action } from 'express-sequelize-crud'
import { User } from './models'

const app = new express()
app.use(
  crud('/admin/users', User, {
    hooks: {
      [Action.GET_LIST]: {
        after: async (records, filters) => doSomething(records, filters),
      },
      [Action.GET_ONE]: {
        after: async record => doSomething(record),
      },
      [Action.CREATE]: {
        before: async body => doSomething(body),
        after: async record => doSomething(record),
      },
      [Action.UPDATE]: {
        before: async (body, record) => doSomething(body, record),
        after: async record => doSomething(record),
      },
    },
  })
)
```

### Search

#### Autocomplete

When using react-admin autocomplete reference field, the searched columns must be specified:

```ts
crud('/admin/users', User, {
  searchableFields: ['email', 'name'],
})
```

When searching `some stuff`, the following records will be returned in this order:

1. records with a searchable field that contains `some stuff`
2. records that have searchable fields that contain both `some` and `stuff`
3. records that have searchable fields that contain one of `some` or `stuff`

The search is case insensitive.

#### Filters

express-sequelize-crud support React Admin searches that _contains_ a string. For instance to search users with emails that end with _@example.com_ one can filter: `%@lalilo.com`.

## contribute

### How to publish a new version on npmjs.org

- update the version in package.json
- tag the commit `git tag -a vx.x.x`
- push
