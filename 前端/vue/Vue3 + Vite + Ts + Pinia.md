## 创建项目
使用`npm init vite`创建项目
## 配置vue-routr
在`src/router/index.ts`中
```typescript
import { createRouter, createWebHashHistory } from 'vue-router'
import routes from './routes'
const router = createRouter({
    history: createWebHashHistory(),
    routes,
})

// 路由守卫
router.beforeEach((to, from, next) => {
    next()
})

export default router
```

在`src/router/routes.ts`中
```typescript
import { RouteRecordRaw } from "vue-router";

const routes: RouteRecordRaw[] = [
    { path: '/', name: 'index', component: () => import('@/views/index.vue') }
]

export default routes
```

在`main.ts`中，引入router并注册
```typescript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'
createApp(App).use(router).mount('#app')

```
## 配置Pinia
### 配置
在`src/store/index.ts`中
```typescript
import { defineStore } from 'pinia'

export const useStore = defineStore('storeId', {
    state: () => {
        return {}
    },
    getters: {},
    actions: {}
})
```

在`main.ts`中引用并注册
```typescript
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia'
createApp(App).use(createPinia()).mount('#app')
```

### 在页面使用
```vue
// page.vue
<script setup>
	import { useStore } from '../store'
	const store = useStore();
	let { name } = store;
</script>
```
### 修改数据
```vue
// page.vue
<script setup>
	import { storeToRefs } from 'pinia'
	import { useStore } from '../store'
	const store = useStore();
	let { aa }  = storeToRefs(store);// storeToRefs是为了能响应式
	
	const btn = ()=>{
		aa.value++;
	}
	
	// 批量更新
	store.$patch(state=> {
		state.aa++
		state.bb++
	})
	
	
</script>
```
### 分模块
```javascript
// store/user.js
export const user = defineStore('user',{
	state: () => {
		return {
			aa:1,
			bb:2
		}
	},
	getters:{},
	actions:{}
})


// store/aa.js
export const aa = defineStore('aa',{
	state: () => {
		return {
			aa:1,
			bb:2
		}
	},
	getters:{},
	actions:{}
})

// page.vue引用
<script setup>
import { user } from '../store/user.js'
import { aa } from '../store/aa.js'
const userStore = user()
const aaStore = aa()
</script>


```
### 持久化存储

1. 安装插件`npm i pinia-plugin-persist --save`
2. 使用
```javascript
// store/index.js
import { createPinia } from 'pinia'
import piniaPluginPersist from 'pinia-plugin-persist'

const store = createPinia()
store.use(piniaPluginPersist)

export default store


// store/user.js
export const useUserStore = defineStore('user',{
	state: () => {
		return {
			name: '张三'
		}
	},
	
	// 开启数据缓存
	persist: {
		enabled: true,
		// 数据默认存在 sessionStorage 里，并且会以 store 的 id 作为 key。
		// 要修改再strategies中修改
		strategies: [
			{
				key: 'my_user',
				storage: localStorage,
				// 默认所有 state 都会进行缓存，你能够通过 paths 指定要长久化的字段，其余的则不会进行长久化。
				paths: ['name']
			}
		]
		
	}
})



```
## 配置Axios
按实际需求配置即可，以下为参考
```typescript
// http/index
import axios, { AxiosRequestConfig } from 'axios';
import { Http } from './httpInterface';

// 接口请求URL
const BASEAPIURL: string = import.meta.env.VITE_BASE_API_URL;
// 接口请求前缀
const PREFIX: string = import.meta.env.VITE_API_PREFIX;

// 设置请求头和请求路径
axios.defaults.baseURL = BASEAPIURL;
axios.defaults.timeout = import.meta.env.VITE_API_TIMEOUT * 1;
axios.defaults.headers.post['Content-Type'] = 'application/json;charset=UTF-8';
axios.defaults.headers.get['Content-Type'] =
  'application/x-www-form-urlencoded';
// 添加请求拦截器
axios.interceptors.request.use(
  function (config): AxiosRequestConfig<any> {
    // 在发送请求之前做些什么
    if (PREFIX) {
      config.url = `${PREFIX}${config.url}`;
    }
    console.log(config);
    return config;
  },
  function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  }
);

// 添加响应拦截器
axios.interceptors.response.use(
  (response) => {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  },
  (error) => {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  }
);

const http: Http = {
  get(url, params) {
    return new Promise((resolve, reject) => {
      axios
        .get(url, { params })
        .then((res) => {
          resolve(res.data);
        })
        .catch((err) => {
          reject(err.data);
        });
    });
  },
  post(url, params) {
    return new Promise((resolve, reject) => {
      axios
        .post(url, JSON.stringify(params))
        .then((res) => {
          resolve(res.data);
        })
        .catch((err) => {
          reject(err.data);
        });
    });
  }
  //   upload(url, file) {
  //     return new Promise((resolve, reject) => {
  //       axios
  //         .post(url, file, {
  //           headers: { 'Content-Type': 'multipart/form-data' }
  //         })
  //         .then((res) => {
  //           resolve(res.data);
  //         })
  //         .catch((err) => {
  //           reject(err.data);
  //         });
  //     });
  //   },
  //   download(url) {
  //     const iframe = document.createElement('iframe');
  //     iframe.style.display = 'none';
  //     iframe.src = url;
  //     iframe.onload = function () {
  //       document.body.removeChild(iframe);
  //     };
  //     document.body.appendChild(iframe);
  //   }
};

export default http;


```

```typescript
// http/httpInterface
export interface ResType<T> {
  code: number;
  data?: T;
  msg: string;
  err?: string;
}

export interface Http {
  get<T>(url: string, params?: unknown): Promise<ResType<T>>;
  post<T>(url: string, params?: unknown): Promise<ResType<T>>;
  //   upload<T>(url: string, params: unknown): Promise<ResType<T>>;
  //   download(url: string): void;
}

```
## 配置sass
![image.png](https://cdn.nlark.com/yuque/0/2022/png/340687/1653036450353-98f42a21-5ef9-437a-a0f4-15e04ab85091.png#averageHue=%23c1c3c5&clientId=ub5a8da2e-33e7-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=679&id=ue08c0495&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1358&originWidth=1922&originalType=binary&ratio=1&rotation=0&showTitle=false&size=284827&status=done&style=none&taskId=udcc007ef-670c-4f6b-9c2b-a4bc465893d&title=&width=961)
[Vite文档](https://cn.vitejs.dev/guide/features.html#css-modules)
## 环境变量配置
新建`.env.development`及`.env.production`文件
```javascript
NODE_ENV=development

VITE_APP_WEB_URL= ' URL' // 要以 VITE 开头 

```
:::danger
为了防止意外地将一些环境变量泄漏到客户端，只有以 VITE_ 为前缀的变量才会暴露给经过 vite 处理的代码
:::
组件内使用环境
`console.log(import.meta.env.VITE_APP_WEB_URL)`

## Vite环境配置
### 路径别名
```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
// 如果path报错，安装@types/node即可
import { resolve } from 'path'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        vue(),
    ],
    resolve: {
				// 路径别名
        alias: {
            '@': resolve(__dirname, 'src'),
            '@api': resolve(__dirname, 'src/api'),
            '@img': resolve(__dirname, 'src/assets'),
            '@comp': resolve(__dirname, 'src/components'),
            '@utils': resolve(__dirname, 'src/utils'),
            '@styles': resolve(__dirname, 'src/styles')
        }
    }
})

```

设置完上面的配置，如果直接使用，是会报错`_can not find module ...._`
还得在`tsconfig.json`设置`paths`
```typescript
// tsconfig.json
{
  "compilerOptions": {
    "target": "esnext",
    "useDefineForClassFields": true,
    "module": "esnext",
    "moduleResolution": "node",
    "strict": true,
    "jsx": "preserve",
    "sourceMap": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "lib": ["esnext", "dom"],
    "skipLibCheck": true,
		// 加入以下内容
    "paths": {
      "@/*": ["./src/*"],
      "@api/*": ["./src/api/*"],
      "@img/*": ["./src/assets/*"],
      "@comp/*": ["./src/components/*"],
      "@utils/*": ["./src/utils/*"],
      "@styles/*": ["./src/styles/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}

```
### 配置自定义代理规则
```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        vue(),
    ],
	server: {
		proxy: {
		  '/api': {
			target: 'http://www.api.com',
			changeOrigin: true,
			// secure: false, //忽略证书检查
			rewrite: (path) => path.replace(/^\/api/, '')
		  }
		}
	}
})

```

动态获取`.ENV` 配置来实现代理
```typescript
		import { defineConfig, loadEnv } from 'vite';
import vue from '@vitejs/plugin-vue';

// https://vitejs.dev/config/
export default defineConfig(({ mode }) => {export default defineConfig(({ mode }) => {
  // 根据当前工作目录中的 `mode` 加载 .env 文件
  // 设置第三个参数为 '' 来加载所有环境变量，而不管是否有 `VITE_` 前缀。
  const ENV = loadEnv(mode, process.cwd(), '');
  return {
    plugins: [
      vue()
    ],
    server: {
      port: 8888,
      proxy: {
        [ENV.VITE_API_PREFIX]: {
          target: ENV.VITE_BASE_API_URL,
          changeOrigin: true,
					// secure: false, //忽略证书检查
          rewrite: (path) => path.replace(ENV.VITE_API_PREFIX, '')
        }
      }
    }
  };
});

```
## Vite插件配置
### 自动导入vue3的hooks
使用[unplugin-auto-import/vite](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fantfu%2Funplugin-auto-import)

安装 `npm i -D unplugin-auto-import`

#### 配置
```typescript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'

// https://vitejs.dev/config/
export default defineConfig({
    plugins: [
        vue(),
        AutoImport({
            imports: ['vue', 'vue-router'],// 按实际需求引入
						// 使用ts需要配置dts,不用ts可注释
            dts: 'src/auto-import.d.ts' 
        }),
        Components({
            resolvers: [
                VantResolver()
            ]
        })
    ]
})

```
#### 使用
```vue
//引入前
<script setup lang='ts'>
import { ref, computed } from 'vue'
const count = ref(0)
const doubled = computed(() => count.value * 2)
</script>


//引入后
<script setup lang='ts'>
const count = ref(0)
const doubled = computed(() => count.value * 2)
</script>
```
### UI组件自动按需引入
使用 [unplugin-vue-components](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fantfu%2Funplugin-vue-components%23readme)插件
		安装`npm install unplugin-vue-components -D`

#### 配置
```typescript
// vite.config.js
import { defineConfig } from 'vite'
import Components from 'unplugin-vue-components/vite'

// 下面是常用的UI库，按实际需求引入即可
import {
  ElementPlusResolver,
  AntDesignVueResolver,
  VantResolver,
  HeadlessUiResolver,
  ElementUiResolver
} from 'unplugin-vue-components/resolvers'

export default defineConfig({
  plugins: [
    Components({
      // ui库解析器，也可以自定义
      resolvers: [
        ElementPlusResolver(),
        AntDesignVueResolver(),
        VantResolver(),
        HeadlessUiResolver(),
        ElementUiResolver()
      ]
    })
  ]
})

```
#### 使用
```vue
<template>
//直接使用，不需要引入
<van-button type="primary">主要按钮</van-button>
</template>

<script lang="ts" setup>
</script>

<style scoped lang="scss"></style>

```
