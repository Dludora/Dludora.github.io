---
title: vite配置mockjs
date: 2022-09-27 00:21:31
tags: "vue"
---

# Vite2 配置mockjs

仓库地址	https://github.com/vbenjs/vite-plugin-mock

## 安装前置依赖

```sh
npm i mockjs -S
npm i vite-plugin-mock -D
npm i cross-env -D
```

`package.json`配置`script > dev`

```sh
"dev": "cross-env NODE_ENV=development vite"
```

`vite.config.ts`

```typescript
import { viteMockServe } from 'vite-plugin-mock'

plugins: [vue(),viteMockServe({mockPath: './mock'})],
```

```tex
supportTs: true`是否用了ts，可以根据自己选择`true` or `false`
```

## 代码

在与`node_modules`同级目录建立mock目录

目录下建立mock文件 `user.ts`

```js
import {faker} from '@faker-js/faker'
import type {User, loginData} from '../../src/types/user'
import type {MockMethod} from "vite-plugin-mock";

const userList: User[] = [
    {
        userId: 0,
        username: 'admin',
        passwd: 'any',
        avatar: 'https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif',
        instruction: 'I am a super administrator',
        email: 'admin@test.com',
        roles: ['admin']
    },
    {
        userId: 1,
        username: 'editor',
        passwd: 'any',
        avatar: 'https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif',
        instruction: 'I am an editor',
        email: 'editor@test.com',
        roles: ['editor']
    },
]

const userCount = 100

for (let i = 2; i < userCount; i++) {
    userList.push({
        userId: i,
        username: 'user_' + faker.random.alphaNumeric(9),
        passwd: faker.random.alphaNumeric(20),
        avatar: faker.image.imageUrl(),
        instruction: faker.lorem.sentence(20),
        email: faker.internet.email(),
        roles: ['visitor']
    })
}

export const register = () => {
    return {
        code: 20000
    }
}

export const login = (body:loginData) => {
    const {username, passwd} = body
    for (const user of userList) {
        if (user.username === username) {
            return {
                code: 20000,
                data: {
                    accessToken: username + '-token'
                }
            }
        }
    }
    return {
        code: 50004,
        message: 'Invalid User'
    }
}

export const logout = () => {
    return {
        code: 20000
    }
}


export default [
    {
        url: '/api/user/login',
        method: 'post',
        response: ({body}: { body: loginData }) => {
            return login(body)
        }
    }
] as MockMethod[]
```
