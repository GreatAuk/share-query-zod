---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
css: unocss
transition: slide-left
title: 技术分享-吴展华
---

# 前端技术分享

<v-clicks>
  <div class="text-left ml-30 mt-20 text-2xl">
    1. @tanstack/vue-query: 用于数据请求的 Vue Hooks 库
  </div>
  <div class="text-left ml-30 py-4 text-2xl">
    2. 表格的封装和一些用户体验优化
  </div>
  <div class="text-left ml-30 text-2xl">
    3. Zod: 一个以 TypeScript-First 的数据验证库
  </div>
</v-clicks>

<div class="abs-bl m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/GreatAuk/share-img-optimize" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>
<div
  v-click
  class="abs-br m-6 flex item-center text-3xl"
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }"
>
  吴展华
  <img src="https://avatars.githubusercontent.com/u/20253809?s=40&v=4" class="w-9 h-9 rounded-full ml-2" />
</div>

---
transition: fade-out
layout: center
---
# @tanstack/vue-query

<div>
  用于数据请求的 Vue Hooks 库。
</div>

---

# stale-while-revalidate

一种由 HTTP RFC 5861 推广的 HTTP 缓存失效策略。

这种策略首先从缓存中返回数据（过期的），同时发送 fetch 请求（重新验证），最后得到最新数据。

---
transition: fade-out
---

# 基本的使用

```ts
<script setup>
import { ref } from 'vue'

const isLoading = ref(false)
const isError = ref(false)
const data = ref<any[]>([])

const getTodos = async() => {
  try {
    isLoading.value = true
    data.value = await axios.get('/api/todos')
  } catch (err) {
    isError.value = true
  } finally {
    isLoading.value = false
  }
}
</script>
<template>
  <span v-if="isLoading">Loading...</span>
  <span v-else-if="isError">Error: {{ error.message }}</span>
  <ul v-else>
    <li v-for="todo in data" :key="todo.id">{{ todo.title }}</li>
  </ul>
</template>
```

---

# 基本的使用

```ts
<script setup>
import { useQuery } from '@tanstack/vue-query'

const getTodos = async() => {
  return axios.get('/api/todos'')
}

// Query
const { isLoading, isFetching, isError, data, error } = useQuery({
  queryKey: ['todos'],
  queryFn: getTodos
})
</script>

<template>
  <span v-if="isLoading">Loading...</span>
  <span v-else-if="isFetching">Fetching...</span>
  <span v-else-if="isError">Error: {{ error.message }}</span>
  <ul v-else>
    <li v-for="todo in data" :key="todo.id">{{ todo.title }}</li>
  </ul>
</template>
```

---

<div v-click>
  原始:
  <img src="https://utopia1994.oss-cn-shanghai.aliyuncs.com/img-bed/202306092308597.png" />
</div>

<div v-click>
  使用 vue-query 后:
  <img src="https://utopia1994.oss-cn-shanghai.aliyuncs.com/img-bed/202306092318460.png" />
</div>

<div v-click class="mt-8">
相比于一般的在组件中发起请求，vue-query 通过 useQuery 帮我们自然而然的生成了一个请求中间层。
</div>
---

<img class="h-50vh m-auto" src="https://utopia1994.oss-cn-shanghai.aliyuncs.com/img-bed/202306092353426.png" />

---

# 特性

<v-clicks>

- 内置缓存，自动垃圾收集
- 重复请求去除，解决竞态的问题
- 窗口聚焦时重新验证（visibilitychange or focus）
- 网络恢复时重新验证
- 错误重试
- 分页查询、无限查询和滚动位置恢复
- 支持 TypeScript
- 轮询/长轮询
- 有依赖的查询
- 查询取消
- 乐观更新
- 离线缓存(实验中)
- 支持 SSR / ISR / SSG
- 开发者工具 (官方 vue devtool 已集成)

</v-clicks>

---
layout: center
---

# 表格的封装和一些用户体验优化

---

# 常见表格形态

<img src="https://gw.alipayobjects.com/zos/antfincdn/Hw%26ryTueTW/bianzu%2525204.png" />

---

# 表格优化总结

<v-clicks>

- 封装组件 TableWrap, 规范、统一布局。
- 封装 hook - useTable, 减少样板代码。
- 分页、筛选条件、搜索条件、排序状态的持久化（同步到 url）。
- 表格中的时间、状态、操作栏需保持词语完整不过行。
- 当页面内容不足一页时，不展示分页器。
- 诸如金额、数量等数值分布排列时，通常采用“右对齐”方式，既方便用户快捷读取数据，还可以使用户进行纵向数据对比。
- 点击删除操作时，需要二次确认。
- 当单元格数据为空时，可使用 - 来表示暂无数据。
- 当列表无数据或无搜索结果时，应展示空状态。

</v-clicks>

<img v-show="$slidev.nav.clicks === 6" class="w-350px absolute bottom-100px right-20px" src="https://gw.alipayobjects.com/mdn/rms_08e378/afts/img/A*vjAcTqS6VKoAAAAAAAAAAABkARQnAQ" />

---
transition: fade-out
layout: center
---
# Zod

<div>
  Zod 是一个以 TypeScript-First 的数据验证库，弥补了 TypeScript 无法在运行时进行校验的问题。

  Zod 既可以用在服务端也可以运行在客户端，以保障 Web Apps。
</div>

---

```ts
// async-validator
const formRules = [
  name: [
    { required: true, message: 'Please input Activity name', trigger: 'blur' },
    { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' },
  ],
  region: [
    { required: true, message: 'Please select Activity zone', trigger: 'change' },
  ]
]
```

<v-click>
```ts
// zod
import { z } from 'zod'

const formSchema = z.object({
  name: z.string()
    .min(3, { message: 'Length should be 3 to 5' })
    .max(5, { message: 'Length should be 3 to 5' })
    .nonempty({ message: 'Please input Activity name' }),
  region: z.string()
    .nonempty({ message: 'Please select Activity zone' }),
});

formSchema.safeParse({name: 'utopia'}); // => { success: false; error: ZodError }
```

</v-click>

---

```ts
const formSchema = z.object({
  id: z.number(),
  name: z.string(),
  region: z.boolean(),
  age: z.number().optional(),
  sex: z.union([z.literal("male"), z.literal("female")]),
  type: z.literal("User")
})
```

<v-click>

```ts
// 根据 formSchema 自动推导出 ts 类型
type FormState = z.infer<typeof formSchema>

// FormState 的 类型
type FormState = {
  id: number;
  name: string;
  region: boolean;
  age?: number | undefined;
  sex: "male" | "female";
  type: "User";
}

```

</v-click>

<v-click>
  <a href="https://transform.tools/typescript-to-zod" target="_blank" class="text-xl">TypeScript to Zod </a>
</v-click>

---

# Zod 的优点
<v-clicks>

- 它很小：8kb 缩小 + 压缩
- 零依赖
- 基于 schema 推荐 ts 类型
- 简洁、可链接的界面
- 支持 Promise 和函数模式
- 也适用于纯 JavaScript！你不需要使用 TypeScript。

</v-clicks>

<div v-click class="mt-10">

> zod 注意事项：
> 1. TypeScript 4.1 及更高版本
> 2. 启用严格模式

</div>

---

# Zod 的使用场景
<v-clicks>

- 表单场景类型校验（移动端、小程序）
- 前端校验后端返回的数据
- 后端接口参数校验
- 配置文件验证：用于验证应用程序的配置文件，例如验证环境变量、JSON 配置文件等。
- 上报参数过滤敏感信息字段

</v-clicks>

---

## 场景一： 通过 Zod 校验环境变量

<div v-show="$slidev.nav.clicks < 1">

```ts

export const envSchema = z.object({
  // 项目本地运行端口号
  VITE_PORT: z.string().optional().refine((v) => {
    if (v) return isNumberLike(v)
    return true
  }, {
    message: "VITE_PORT must be a number"
  }).transform(Number),

  // Base public path when served in development or production
  VITE_PUBLIC_PATH: z.string().startsWith('/').default("/"),

  // 开发环境路由历史模式（Hash模式传"hash"、HTML5模式传"h5"、Hash模式带base参数传"hash,base参数"、HTML5模式带base参数传"h5,base参数"）
  VITE_ROUTER_HISTORY: z.union([z.literal("h5"), z.literal("hash")]),

  // 本地跨域代理匹配的 prefix
  VITE_PROXY_DOMAIN: z.string().optional(),

  // 线上环境后端地址
  VITE_PROXY_DOMAIN_REAL: z.string().url()
})
.strict() // 严格模式，如果环境变量中存在任何未知的 keys，Zod 将抛出一个错误。
```

</div>

<v-click>

```ts
// vite.config.ts
import { defineConfig, loadEnv } from 'vite'
import vue from '@vitejs/plugin-vue'

import { envSchema } from './envSchema'

const root = process.cwd()

// https://vitejs.dev/config/
export default defineConfig(({ mode }) => {
  const envs = loadEnv(mode, root) as ImportMetaEnv

  const { VITE_PUBLIC_PATH } = envSchema.parse(envs)

  return {
    base: VITE_PUBLIC_PATH,
    plugins: [
      vue(),
    ],
  }
})

```

</v-click>

---
layout: end
---

# END
