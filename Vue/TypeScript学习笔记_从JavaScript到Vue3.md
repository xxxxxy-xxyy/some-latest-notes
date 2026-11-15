
## 第1章 为什么需要 TypeScript

### 1.1 先回答：学 Vue 前一定要先学完 TS 吗

不需要先把 TS 全部学完。Vue 可以使用 JavaScript，也可以使用 TypeScript。

对你更合适的顺序是：

1. 掌握 JS 的函数、对象、数组、解构、模块和异步。
2. 学习 TS 基础类型、类型推断、interface/type、联合类型与泛型。
3. 开始学习 Vue，在组件、表单和接口请求中继续补 TS。
4. 项目遇到需求时，再深入复杂工具类型、条件类型和库的类型声明。

TS 很值得补，但不必把“学完 TS”设成开始 Vue 的硬性门槛。现阶段优先追求：看得懂常见类型、能给接口数据和组件参数定义类型、能定位报错。

### 1.2 TS 解决的典型问题

JavaScript 示例：

```js
function calculateTotal(price, quantity) {
  return price * quantity;
}

calculateTotal('abc', 2); // 运行后得到 NaN，问题可能很晚才暴露
```

TypeScript 示例：

```ts
function calculateTotal(price: number, quantity: number): number {
  return price * quantity;
}

calculateTotal(99, 2);
// calculateTotal('abc', 2); // 类型错误：string 不能作为 number 实参
```

TS 的价值包括：提前发现部分错误、补全属性和参数、表达数据契约、帮助重构、让团队代码更易理解。

它不会自动修复业务逻辑、网络失败、权限漏洞，也不能证明任意代码绝对正确。

### 1.3 TypeScript 与 JavaScript 的关系

可以把 TS 理解为：建立在 JavaScript 之上、带有静态类型检查与相关语法的语言。

```ts
const username: string = '小轩';
```

在常见前端构建流程中，类型注解会被去掉，运行的是 JavaScript：

```js
const username = '小轩';
```

要分清三件事：

| 阶段 | 主要工作 | 是否等于运行时数据验证 |
| --- | --- | --- |
| 类型检查 | 根据源码与声明检查类型关系 | 否 |
| 转译/构建 | 去除类型、转换语法、打包等 | 否 |
| 运行 | 浏览器或其他环境执行 JS 逻辑 | 只有你写的运行时检查才会实际执行 |

多数 TS 类型信息会被擦除；interface、type、泛型参数通常不保留为运行时可查询的类型对象。某些 TS 语法会生成运行时代码，例如普通 enum、参数属性等，不能笼统认为所有 TS 新语法都只是删除。

部分运行时支持直接处理一定范围的 TS 语法，但不代表浏览器可直接执行任意 .ts，更不代表执行时做了完整类型检查。学前端先按项目工具链使用 TS。

### 1.4 最重要的边界：类型声明不验证接口数据

```ts
interface User {
  id: string;
  name: string;
}

const text = '{"id":123,"name":null}';
const data = JSON.parse(text) as User;
// 编译器被告知按 User 看待；真实数据并没有因此变成 User
// data.name.toUpperCase(); // 若运行，会因为 name 是 null 出错
```

`as User` 不是解析器，也不是验证器。TS 很擅长检查“在可信类型假设下，代码是否一致”，但数据边界上的假设需要由运行时校验、后端契约、生成工具等共同保证。

后面会用 unknown 与类型守卫演示更可靠的数据处理。

### 1.5 与 Java 的相似点和差异

| 维度 | Java | TypeScript |
| --- | --- | --- |
| 静态类型 | 编译阶段检查 | 开发/编译阶段检查 |
| 类型注解位置 | 常在变量名前 | 常写在变量名后，如 name: string |
| 对象兼容性 | 通常重视声明的类/接口关系 | 主要按结构比较，存在私有成员等例外 |
| 接口 | 常由类显式 implements | 普通对象结构满足即可赋值 |
| 泛型 | 表达参数化类型 | 表达输入输出、属性等类型关系，通常运行时擦除 |
| 空值 | 引用类型可为 null，另有生态工具约束 | strictNullChecks 下显式区分可空与非空 |
| 数字 | int、long、double 等 | 常用 number；BigInt 是另一种 JS 数值类型 |
| 运行时模型 | JVM 与 Java 对象模型 | 保留 JS 的对象、原型、闭包、异步等语义 |

不要因为看到 class、interface、泛型，就把 TS 当作“浏览器里的 Java”。特别要记住：TS 的对象形状和 JS 的运行时行为是理解主线。

---

## 第2章 环境、编译与 tsconfig

### 2.1 几个文件扩展名

| 扩展名 | 常见用途 |
| --- | --- |
| .js | JavaScript 源码 |
| .ts | TypeScript 源码 |
| .tsx | 包含 JSX 的 TypeScript 源码；普通 Vue SFC 不需要改成它 |
| .d.ts | 类型声明文件，描述现有代码的类型 |
| .vue | Vue 单文件组件，可包含 script setup lang="ts" |

.d.ts 里的声明不能凭空提供真正的运行时函数，后面会专门说明。

### 2.2 最小安装与练习

需要先安装 Node.js 与 npm，在新建练习目录中执行：

```bash
npm init -y
npm install --save-dev typescript
npx tsc --version
```

新建 `src/main.ts`：

```ts
const topic: string = 'TypeScript';
const hours: number = 2;
console.log(`今天学习 ${topic}，共 ${hours} 小时`);
```

再新建下面的 tsconfig.json，运行编译与 JS 输出。

### 2.3 一个用于入门的 tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "strict": true,
    "noEmitOnError": true,
    "rootDir": "src",
    "outDir": "dist"
  },
  "include": ["src/**/*.ts"]
}
```

```bash
npx tsc -p tsconfig.json
node dist/main.js
```

这份配置是用于现代前端语境的入门练习，main.ts 没有模块导入。第11章会明确设置 package.json 的 type，以运行带 ESM 导入导出的练习。

配置说明：

| 选项 | 含义 | 常见误解 |
| --- | --- | --- |
| target | 控制输出 JS 的语法目标等 | 不会自动为所有新 API 补 polyfill |
| module | 输出模块形式 | 不等于 JS 引擎天然认识所有包名 |
| moduleResolution | TS 如何查找模块与声明 | 不会替代运行时/打包器的解析规则 |
| lib | 提供哪些标准 API 类型声明 | 加 DOM 不会在 Node 中创造 document |
| strict | 开启一组严格检查 | 不等于绝对没有类型漏洞 |
| noEmitOnError | 有检查错误时不输出文件 | 不加时有些流程仍可能产生 JS |
| rootDir / outDir | 源码与输出目录 | 不应把输出重新纳入输入 |
| include | 纳入项目的文件范围 | 不是运行时路由或文件访问控制 |

Vue 工程应优先沿用官方脚手架提供的分层配置，不要把上面这个独立练习配置覆盖到已有 Vue 项目。

### 2.4 类型检查与输出可以分开

```bash
# 使用项目配置，只检查，不生成 JS
npx tsc -p tsconfig.json --noEmit
```

注意：`npx tsc src/main.ts` 直接传文件时，不按通常方式读取项目 tsconfig；想使用项目配置，应使用 `tsc -p ...`。不能在同一命令里把 -p 与源码文件列表混用。

对于 Vue 单文件组件，项目通常使用 vue-tsc 进行包含模板与组件的类型检查。Vite 等工具能够转译 TS，并不意味着仅运行开发服务器就已经完成所有类型检查；脚手架的 build 脚本是否额外执行检查，需要看实际 package.json。

### 2.5 严格模式中的重点选项

| 选项 | 学习重点 |
| --- | --- |
| noImplicitAny | 不能把某些缺失类型信息的问题悄悄变成隐式 any |
| strictNullChecks | null/undefined 与非空类型区分，使用前需要处理 |
| strictFunctionTypes | 检查函数参数兼容性中的重要情况，方法等存在例外 |
| strictPropertyInitialization | 检查类实例属性的初始化等 |
| useUnknownInCatchVariables | catch 变量默认按 unknown 处理，先判断再使用 |

可以进一步考虑两个不由 strict 自动全部覆盖的选项：

- `noUncheckedIndexedAccess`：许多数组/字典按索引访问的结果会额外包含 undefined。
- `exactOptionalPropertyTypes`：更严格地区分“属性不存在”与“属性存在但值为 undefined”。

开关会影响部分示例的类型表现，排错时先确认项目配置。本文涉及这些差异时会说明，不通过关掉 strict 解决普通空值问题。

参考：[官方 tsconfig 说明](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)。

---

## 第3章 基础类型、推断与字面量

### 3.1 类型注解与类型推断

```ts
let username: string = '小轩';
let age = 20; // 推断为 number
const enabled = true; // 常量值具有更窄的字面量类型

age = 21;
// age = '21'; // 类型错误
```

不需要给每个显而易见的变量都标注类型。推荐在函数边界、外部数据、空容器、公共接口等地方明确表达，其余地方利用推断。

### 3.2 常见原始类型

```ts
let title: string = '学习 Vue';
let progress: number = 0.5;
let done: boolean = false;
let missing: undefined = undefined;
let empty: null = null;
let hugeId: bigint = 9007199254740993n;
let key: symbol = Symbol('key');
```

使用小写 string、number、boolean。大写 String、Number、Boolean 是包装对象相关类型，日常业务数据不要拿来替代原始类型。

number 没有变成 Java 的 int 或 long。小数精度、安全整数范围等 JS 问题仍然存在，后端长整型 ID 通常可通过字符串契约避免前端精度丢失。

### 3.3 数组、只读数组与元组

```ts
const scores: number[] = [80, 90];
const names: Array<string> = ['HTML', 'CSS'];
const values: (string | number)[] = ['A', 1];

const readonlyScores: readonly number[] = [80, 90];
// readonlyScores.push(100); // 类型错误

const point: [number, number] = [10, 20];
const result: [success: boolean, message: string] = [true, '保存成功'];
```

- 数组描述一组元素的类型。
- 元组额外表达位置、长度与各位置类型，适合有固定含义的位置组合。
- 命名元组元素有助于阅读，不会把位置改成对象属性。
- readonly 只约束当前类型视角下的写入，不自动执行 Object.freeze。

空数组的推断受上下文影响；某些位置可能得到 never[] 等不符合预期的结果。先知道元素类型时可明确标注：

```ts
interface Task { id: string; title: string; }
const tasks: Task[] = [];
```

### 3.4 字面量类型与联合类型

```ts
type Status = 'todo' | 'doing' | 'done';
let status: Status = 'todo';
status = 'done';
// status = 'finished'; // 类型错误

let id: string | number = '1001';
id = 1001;
```

`|` 表示可能是其中一种类型，并不表示可以不加判断地同时使用所有类型的方法。

```ts
function formatId(id: string | number): string {
  return typeof id === 'number' ? id.toFixed(0) : id.trim();
}
```

### 3.5 null、undefined 与可空数据

```ts
interface User { id: string; name: string; }
let currentUser: User | null = null;

function displayName(user: User | null): string {
  return user?.name ?? '未登录';
}
```

“目前没有用户”是正常业务状态，应该让类型表达这个状态，而不是先写 User 再到处用断言压掉错误。

可选参数和可选属性在读取时通常需要处理 undefined。可选链只处理空值，不代表完成所有数据验证。

### 3.6 any 与 unknown

```ts
let unsafe: any = 123;
// unsafe.notExists.run(); // 编译器通常不阻止；运行可能直接报错

let safe: unknown = 123;
// safe.toFixed(2); // 类型错误，使用前需要收窄
if (typeof safe === 'number') {
  console.log(safe.toFixed(2));
}
```

| 类型 | 直观理解 | 使用限制 |
| --- | --- | --- |
| any | 基本退出此处的类型检查 | 容易传播，让下游也失去保障 |
| unknown | 还不知道真实类型 | 必须判断、验证或作有依据的断言后使用 |

外部 JSON、任意捕获错误、第三方不可信输入，通常优先考虑 unknown。any 并不是永远不能出现，但不应成为遇到报错的默认答案。

### 3.7 void、never 与 object

```ts
function logMessage(message: string): void {
  console.log(message);
}

function fail(message: string): never {
  throw new Error(message);
}

function repeatForever(): never {
  while (true) { /* 永不正常返回 */ }
}
```

| 类型 | 含义 |
| --- | --- |
| void | 调用方不应依赖返回结果；不是所有语境下都等同 undefined |
| never | 不可能出现的值；常用于永不正常返回或穷尽检查 |
| object | 非原始类型，不描述具体属性 |
| {} | 非 null/undefined 的值范围，不能简单理解为“空对象” |

例如 `const x: {} = 123` 通常合法；想表达用户对象应定义明确字段，而不是用 {} 代替。

函数返回 void 的细节：一个上下文返回类型为 void 的回调，可以接收某些实际上返回值的函数，调用方忽略其结果。这不等于你能在显式标注 `function f(): void` 的函数体中随意返回数字。

### 3.8 类型拓宽、as const 与 satisfies

```ts
let a = 'todo';   // 通常为 string
const b = 'todo'; // 'todo'
const config = { status: 'todo' }; // status 通常拓宽为 string

const frozenView = { status: 'todo', size: 10 } as const;
// 推断保留字面量，并形成 readonly 属性视角
```

as const 不会在运行时冻结对象，特别是引用到外部可变对象时，不能把它当成通用深度不可变机制。

`satisfies` 用来检查表达式与某个类型约束是否兼容，同时通常保留更具体、有用的表达式类型信息；其上下文也可能影响推断，不能理解为完全不参与推断。

```ts
type Status = 'todo' | 'done';
const statusLabels = {
  todo: '待办',
  done: '完成'
} satisfies Record<Status, string>;

console.log(statusLabels.done.toUpperCase());
```

satisfies 与 as 不同：前者检查是否满足约束，后者主要是告诉编译器如何看待值。两者都不是运行时数据验证。satisfies 自 TS 4.9 起提供，旧项目需确认编译器支持。

---

## 第4章 对象类型、interface 与 type

### 4.1 用类型描述业务数据

```ts
interface User {
  id: string;
  name: string;
  age: number;
  avatarUrl?: string;
  readonly createdAt: string;
}

const user: User = {
  id: 'u1001',
  name: '小轩',
  age: 20,
  createdAt: '2026-11-05T08:00:00Z'
};

console.log(user.avatarUrl ?? '默认头像');
// user.createdAt = 'other'; // 类型错误
```

`?` 表示属性可选，读取时通常包含 undefined；readonly 表示通过这个类型不允许给该属性重新赋值。

readonly 是浅层类型约束，不是运行时访问控制。只读属性里面的可变对象仍可能被修改；其他可变别名也可能修改同一对象。

### 4.2 可选属性不等于自动默认值

```ts
interface SearchOptions {
  keyword: string;
  page?: number;
}

function normalizeOptions(options: SearchOptions) {
  return {
    keyword: options.keyword.trim(),
    page: options.page ?? 1
  };
}
```

类型中的 `page?: number` 不会自动在运行时填入 1，默认行为由代码实现。

启用 exactOptionalPropertyTypes 后，`page?: number` 对“显式写 page: undefined”的接受情况会更严格；如果业务允许显式 undefined，需要把它写进属性值类型。

### 4.3 type 别名

```ts
type UserId = string;
type TaskStatus = 'todo' | 'doing' | 'done';

type Task = {
  id: string;
  title: string;
  status: TaskStatus;
};

type SubmitHandler = (task: Task) => void;
```

type 不会自动创造独立的名义类型。例如 `type UserId = string` 仍与普通 string 兼容，不会自动阻止把订单 ID 传给用户 ID。

需要更严格区分时可以进一步学习品牌类型，但不必现在到处加品牌标记。

### 4.4 interface 与 type 怎么选

| 能力 | interface | type |
| --- | --- | --- |
| 描述对象结构 | 可以 | 可以 |
| 描述联合、原始类型别名、元组等 | 通常不直接用于这些 | 可以 |
| 扩展 | extends | 交叉类型等方式 |
| 同名声明合并 | 支持特定合并规则 | 不支持同名重新声明 |
| 日常 DTO、组件参数 | 适合 | 也适合 |

没有必要把两者人为分成绝对高下。可以形成统一约定：对象契约常用 interface，状态联合和类型计算常用 type；已有项目的约定更重要。

### 4.5 继承、交叉与组合

```ts
interface BaseEntity {
  id: string;
  createdAt: string;
}

interface User extends BaseEntity {
  name: string;
}

type WithPermission = { permissions: string[] };
type AuthorizedUser = User & WithPermission;
```

`&` 要同时满足两边的约束，不是运行时合并操作。

```ts
type A = { id: string };
type B = { id: number };
type Impossible = A & B; // id 需要同时满足 string 与 number，通常变成 never
```

若需要运行时合并对象，仍要写 `{ ...a, ...b }` 等 JS 代码，且要注意覆盖顺序。

### 4.6 结构类型与额外属性检查

```ts
interface Named { name: string; }

const fullUser = { name: '小轩', age: 20 };
const named: Named = fullUser; // 结构满足，可接受

// const direct: Named = { name: '小轩', age: 20 };
// 新鲜对象字面量会触发额外属性检查
```

TS 主要关注目标所需结构是否满足。对象字面量的额外属性检查有助于发现拼写错误，但不能因此理解成“所有 Named 对象运行时只能有 name 一个键”。

这也意味着 DTO 类型不是过滤器：给一个对象赋予某种静态类型，不会自动删除额外字段。发请求前若只允许部分字段，应主动构造请求体。

### 4.7 字典、索引签名与 Record

```ts
interface ScoreMap {
  [subject: string]: number;
}

const scores: ScoreMap = { html: 90, css: 85 };

type Status = 'todo' | 'doing' | 'done';
const labels: Record<Status, string> = {
  todo: '待办',
  doing: '进行中',
  done: '完成'
};
```

Record<有限键联合, 值类型> 很适合要求每个状态都有映射。

`Record<string, number>` 不能保证任意字符串键运行时都存在；未启用 noUncheckedIndexedAccess 时，静态访问结果可能显得过于乐观。字典缺失值应根据业务明确处理。

### 4.8 索引访问类型

```ts
interface User {
  id: string;
  name: string;
  roles: string[];
}

type UserId = User['id'];
type UserRole = User['roles'][number];
type UserSummaryValue = User['id' | 'name'];
```

方括号位于类型位置时，是从类型里提取属性类型，不是在读取某个运行时对象。

---

## 第5章 函数、回调与泛型

### 5.1 参数、返回值与函数类型

```ts
function add(a: number, b: number): number {
  return a + b;
}

const multiply = (a: number, b: number): number => a * b;

type Calculator = (a: number, b: number) => number;
const subtract: Calculator = (a, b) => a - b;
```

函数类型中的参数名主要帮助阅读，不要求实现函数使用完全相同的局部参数名。

返回值通常可以推断。对公共 API、复杂分支、明确业务契约，显式标注返回类型更有价值。

### 5.2 可选参数、默认参数与剩余参数

```ts
function greet(name: string, prefix?: string): string {
  return `${prefix ?? '你好'}，${name}`;
}

function getPage(page = 1): number {
  return page;
}

function sum(...values: number[]): number {
  return values.reduce((total, value) => total + value, 0);
}
```

可选参数通常放在必选参数后。默认参数仍遵循 JS 规则：省略或传 undefined 会使用默认值，传 null 不会自动使用默认值。

函数可接受较少参数这一兼容规则在回调中很常见，例如 map 的回调可以只写自己需要的 item，不必把 index、array 都列出来。

### 5.3 解构参数的标注位置

```ts
interface CreateTaskInput {
  title: string;
  priority?: number;
}

function createTitle({ title, priority = 1 }: CreateTaskInput): string {
  return `[${priority}] ${title.trim()}`;
}
```

注意 `{ title: string }` 在普通 JS 解构参数里可能是在把 title 重命名为名叫 string 的变量。类型应标在整个解构模式后面，不要把重命名语法当作类型注解。

### 5.4 泛型：保留输入与输出的类型关系

```ts
function identity<T>(value: T): T {
  return value;
}

const numberResult = identity(123); // 保留推断出的数值类型信息
const stringResult = identity('Vue');
const explicit = identity<string>('TypeScript');
```

T 是类型参数，不是固定关键字。可以叫 Item、Data 等有含义的名字。

为什么不用 any：

```ts
function looseIdentity(value: any): any {
  return value;
}
```

any 版本丢失输入输出之间的类型约束；泛型版本能把传入类型关联到返回值。

### 5.5 泛型数组与可能不存在的结果

```ts
function first<T>(items: readonly T[]): T | undefined {
  return items[0];
}

const value = first([10, 20]);
if (value !== undefined) {
  console.log(value.toFixed(2));
}
```

这里明确返回 undefined，因为空数组确实没有第一项。不要为了看起来简洁写成 T 并用断言掩盖真实空值。

### 5.6 泛型约束 extends

```ts
function getId<T extends { id: string }>(value: T): string {
  return value.id;
}

getId({ id: 'u1', name: '小轩' });
// getId({ name: '小轩' }); // 类型错误
```

`extends` 在这里表达类型约束：T 至少满足拥有字符串 id 的结构，不是在运行时创建继承关系。

### 5.7 keyof 与安全属性访问

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { id: 'u1', age: 20 };
const age = getProperty(user, 'age'); // number
// getProperty(user, 'email'); // 类型错误
```

| 语法 | 含义 |
| --- | --- |
| keyof T | T 的可用键组成的类型；可能涉及 string、number、symbol |
| K extends keyof T | 限定 K 必须是 T 的某种键 |
| T[K] | K 对应属性的值类型 |

`Object.keys(obj)` 返回的通常是 string[]，不会无条件变成 `(keyof T)[]`，因为运行时对象可能具有静态类型未表达的额外键。不要为消除报错就在所有地方强制断言。

### 5.8 泛型接口与默认类型参数

```ts
interface PageResult<T> {
  items: T[];
  total: number;
  page: number;
  pageSize: number;
}

interface ApiResponse<T = unknown> {
  code: number;
  message: string;
  data: T;
}

interface User { id: string; name: string; }
type UserPageResponse = ApiResponse<PageResult<User>>;
```

这类似 Java 中 Result<Page<User>> 的阅读方式，但仍然只是静态契约，接口响应需要符合它。

优先为真正存在的输入输出关系引入泛型，不要为了显示高级而给所有函数堆多个类型参数。

### 5.9 函数重载

```ts
function normalize(value: string): string;
function normalize(value: string[]): string[];
function normalize(value: string | string[]): string | string[] {
  return typeof value === 'string'
    ? value.trim()
    : value.map(item => item.trim());
}

const a = normalize(' Vue ');
const b = normalize([' HTML ', ' CSS ']);
```

重载签名向调用方描述多种调用方式，后面只有一个实现；不是 Java 那样提供多个同名函数体。

若调用方持有 string | string[] 联合值，上面两个单独重载未必能直接接受它，需要增加对应签名或调整 API。能用清晰的联合参数表达时，也不必强行重载。

### 5.10 Promise 与 async 函数类型

```ts
interface User { id: string; name: string; }

async function getDemoUser(): Promise<User> {
  return { id: 'u1', name: '小轩' };
}

async function run(): Promise<void> {
  const user = await getDemoUser();
  console.log(user.name);
}
```

Promise<User> 描述成功结果类型，不表示拒绝值也被严格限定为某个 Error 类型。catch 后仍应按 unknown 的思路处理错误。

---

## 第6章 类型收窄、类型守卫与断言

### 6.1 为什么联合类型要先判断

```ts
function printValue(value: string | number): void {
  // value.toUpperCase(); // number 没有这个方法
  if (typeof value === 'string') {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

TS 分析条件和控制流后，会把当前位置的类型缩小到更具体的范围，这就是 narrowing，类型收窄。

### 6.2 常见收窄方式

```ts
function formatDate(value: Date | string): string {
  return value instanceof Date ? value.toISOString() : value.trim();
}

function printList(value: unknown): void {
  if (Array.isArray(value)) {
    console.log(value.length);
  }
}

function display(value: string | null): string {
  if (value === null) return '缺失';
  return value.toUpperCase();
}
```

| 方式 | 常见作用 |
| --- | --- |
| typeof | 判断 string、number、boolean 等 |
| instanceof | 判断运行时构造函数关系，如 Date、Error、DOM 元素 |
| in | 检查对象是否具有某属性，配合类型信息收窄 |
| Array.isArray | 判断运行时是否为数组，但不校验每个元素 |
| 相等判断 | 排除 null/undefined 或限定字面量 |
| 提前 return / throw | 使剩余路径具有更明确类型 |

`typeof null === 'object'` 的 JS 历史行为仍然存在，判断对象时应额外排除 null。interface 和 type 在运行时不存在，不能写 `value instanceof User` 来检查 interface User。

### 6.3 真值收窄的业务陷阱

```ts
function showCount(count: number | undefined): string {
  if (count !== undefined) return `数量：${count}`;
  return '未知';
}
```

若写 `if (count)`，会把合法的 0 一起当成假值。处理空字符串、0 等业务值时，应明确判断缺失条件，而不是一律真值判断。

### 6.4 可辨识联合：让状态更准确

不理想的状态模型：

```ts
interface LooseState {
  loading: boolean;
  data?: string[];
  error?: string;
}
```

它允许“loading=true 但同时有 error 和 data”等含糊组合。

更明确的模型：

```ts
type LoadState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; message: string };

function describe(state: LoadState<string[]>): string {
  switch (state.status) {
    case 'idle': return '尚未加载';
    case 'loading': return '加载中';
    case 'success': return `共 ${state.data.length} 条`;
    case 'error': return state.message;
  }
}
```

各分支共享 status 字段，取值是不同字面量，TS 就能根据 status 判断哪些字段存在。它很适合页面请求状态、消息事件和操作结果。

### 6.5 never 与穷尽检查

```ts
type Status = 'todo' | 'doing' | 'done';

function assertNever(value: never): never {
  throw new Error(`未处理的值：${String(value)}`);
}

function label(status: Status): string {
  switch (status) {
    case 'todo': return '待办';
    case 'doing': return '进行中';
    case 'done': return '完成';
    default: return assertNever(status);
  }
}
```

以后给 Status 新增 archived，却忘记补 case 时，assertNever(status) 会帮助暴露遗漏。它仍包含运行时抛错，便于意外数据进入时发现问题。

### 6.6 自定义类型守卫

```ts
interface User {
  id: string;
  name: string;
}

function isRecord(value: unknown): value is Record<string, unknown> {
  return typeof value === 'object' && value !== null && !Array.isArray(value);
}

function isUser(value: unknown): value is User {
  return isRecord(value) &&
    typeof value.id === 'string' &&
    typeof value.name === 'string';
}

const raw: unknown = JSON.parse('{"id":"u1","name":"小轩"}');
if (isUser(raw)) {
  console.log(raw.name.toUpperCase());
}
```

`value is User` 是类型谓词。编译器会信任你写的守卫签名，因此函数体必须真正检查必要条件，不能直接 `return true`。

这个 isRecord 是帮助检查普通业务 JSON 的辅助函数，不是证明任意对象都拥有某种特定原型、无访问器或无其他行为的通用工具。面对复杂来源，要按输入边界设计验证。

真实项目对象层级复杂时，可以考虑专门的运行时校验方案。关键是理解：验证逻辑实际运行，type/interface 本身不运行。

### 6.7 断言函数

```ts
function assertString(value: unknown): asserts value is string {
  if (typeof value !== 'string') {
    throw new Error('期望字符串');
  }
}

function printUpper(value: unknown): void {
  assertString(value);
  console.log(value.toUpperCase());
}
```

断言函数失败时抛错，成功返回后可收窄类型。它与单纯的 `as string` 不同，因为这里真的执行了检查。

### 6.8 as、非空断言与类型转换

```ts
const input = document.querySelector('#name') as HTMLInputElement | null;
const text = '123';
const numberValue = Number(text); // 运行时转换
```

| 写法 | 做了什么 | 不会做什么 |
| --- | --- | --- |
| value as User | 告诉编译器按某类型看待 | 不校验、不修复数据 |
| value! | 告诉编译器此处不为空 | 不阻止运行时出现 null/undefined |
| Number(value) | 实际执行数值转换 | 不保证结果不是 NaN |
| isUser(value) | 执行你写的检查并提供收窄 | 检查是否可靠取决于实现 |

不要把 `as unknown as SomeType` 当成通用排错方式。这类双重断言可能直接跨过本该修正的类型不一致。

在 DOM 场景中，能使用 instanceof 验证实际元素类型时，比无条件强行断言更稳妥。

---

## 第7章 常用工具类型与类型操作

### 7.1 从现有模型派生类型

```ts
interface User {
  id: string;
  name: string;
  email?: string;
  createdAt: string;
}

type UserPatch = Partial<User>;
type FullUser = Required<User>;
type UserView = Readonly<User>;
type UserSummary = Pick<User, 'id' | 'name'>;
type CreateUserInput = Omit<User, 'id' | 'createdAt'>;
```

| 工具类型 | 作用 | 场景 |
| --- | --- | --- |
| Partial<T> | 所有属性变可选 | 局部更新形状 |
| Required<T> | 去除可选标记 | 表达某些字段应存在的视角 |
| Readonly<T> | 属性变只读 | 只读输入或展示视角 |
| Pick<T, K> | 选取指定属性 | 列表摘要、部分视图 |
| Omit<T, K> | 去掉指定属性 | 创建参数、隐藏服务端字段 |
| Record<K, V> | 键到值的映射类型 | 状态文案、配置字典 |

这些工具通常是浅层操作，不会递归到任意嵌套对象，也不会执行运行时处理。

`Required<T>` 不会自动填字段；`Omit<T, 'password'>` 不会自动删除运行时对象上的 password；`Readonly<T>` 不会冻结对象。

### 7.2 更新 DTO 不能一律 Partial<Entity>

```ts
interface User {
  id: string;
  name: string;
  role: 'user' | 'admin';
  createdAt: string;
}

type UpdateProfileInput = Partial<Pick<User, 'name'>>;
```

若直接 Partial<User>，会把 id、role、createdAt 都放进可选更新参数。哪些字段允许用户修改，应由业务明确决定，后端还必须独立校验。

类型复用的目标是表达正确契约，不是尽可能少写几个字符。

### 7.3 联合类型的集合操作

```ts
type Status = 'todo' | 'doing' | 'done' | null;
type ActiveStatus = Exclude<Status, 'done' | null>;
type FinalStatus = Extract<Status, 'done'>;
type PresentStatus = NonNullable<Status>;
```

| 工具 | 含义 |
| --- | --- |
| Exclude<T, U> | 从联合 T 中排除可赋给 U 的成员 |
| Extract<T, U> | 从联合 T 中提取可赋给 U 的成员 |
| NonNullable<T> | 从 T 中排除 null 和 undefined |

这些都是类型计算，不会把一个数组里的元素自动筛掉，也不会在运行时阻止传入非法数据。

### 7.4 从函数提取类型

```ts
function makeUser(name: string, age: number) {
  return { name, age, active: true };
}

type MakeUserArgs = Parameters<typeof makeUser>;
type MadeUser = ReturnType<typeof makeUser>;

async function loadName(): Promise<string> {
  return '小轩';
}
type LoadedName = Awaited<ReturnType<typeof loadName>>;
```

- Parameters 得到参数元组。
- ReturnType 得到返回值类型。
- Awaited 帮助表达 await 后的结果类型。
- `typeof makeUser` 位于类型位置时，提取函数值的静态类型。

函数有复杂重载时，这些工具的结果会受到重载签名规则影响，不要把简单例子推广到所有库函数。

### 7.5 类型位置的 typeof 与 JS 的 typeof

```ts
const config = { baseUrl: '/api', timeout: 5000 };
type Config = typeof config; // 类型运算
console.log(typeof config);  // 运行时字符串 'object'
```

名字相同，所处位置不同。类型位置用于构造类型；表达式位置执行 JS 的运行时操作。

### 7.6 映射类型：知道工具类型怎么来的

```ts
type OptionalProperties<T> = {
  [K in keyof T]?: T[K];
};

type ReadonlyProperties<T> = {
  readonly [K in keyof T]: T[K];
};
```

可以理解为在类型层面遍历每个键，再构造新的属性规则。它不是 for 循环，也不会产生运行时对象。

业务开发初期会用内置 Partial、Readonly 就够了，理解原理有助于读库类型。

### 7.7 条件类型、infer 与模板字面量类型

```ts
type ItemOf<T> = T extends readonly (infer Item)[] ? Item : never;
type NumberItem = ItemOf<number[]>; // number

type Size = 'small' | 'large';
type EventName = `change:${Size}`; // 'change:small' | 'change:large'
```

条件类型表达“类型满足条件时得到什么类型”；infer 在匹配中提取类型。它们的分配规则、递归开销和边界更复杂，第一轮先认识即可。

建议先会用接口、联合、泛型和内置工具类型，再投入复杂类型体操。真实项目可读性比写出一大段难以维护的类型计算更有价值。

参阅：[官方 Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)、[Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)。

---

## 第8章 模块、声明文件、类与枚举

### 8.1 类型也可以导出与导入

文件 `types.ts`：

```ts
export interface User {
  id: string;
  name: string;
}

export type UserStatus = 'active' | 'disabled';
```

另一个文件中：

```ts
import type { User, UserStatus } from './types.js';

const user: User = { id: 'u1', name: '小轩' };
const status: UserStatus = 'active';
console.log(user, status);
```

这里写 .js 是为了表达输出后模块路径的约定，TS 在相应解析规则下可以定位同名 .ts 源码。Vue/Vite 源码也常见无扩展名导入，具体规则取决于项目工具链；不要随意混用浏览器、Node 与打包器的模块配置。

import type 明确表示只导入类型，通常不会在 JS 输出中保留运行时导入。

类可能同时是类型与运行时值：如果要 `new UserClass()`，就不能只用 import type 导入这个类。

### 8.2 类型空间与值空间

```ts
interface User {
  name: string;
}

const user: User = { name: '小轩' };
// console.log(User); // 错误：interface 不是运行时值
```

类型描述“怎样看待值”，实际值才参与运行时计算。弄清这个区别后，很多“为什么 interface 不能 instanceof”的疑惑就能解决。

### 8.3 .d.ts 与 declare

一个用于解释的声明：

```ts
declare const __APP_VERSION__: string;
```

它告诉 TS：运行环境会提供这个变量。它不会自己在浏览器创建 __APP_VERSION__。如果构建工具或运行环境没有真正注入，运行时仍会报变量不存在。

第三方库类型可能来自：库自身携带的声明，或独立的 @types 包。缺少类型时，应先检查库是否自带声明与导入路径是否正确，再考虑合适的 @types；不是每个包都有对应 @types。

还要区分：

- DOM 类型描述浏览器 API。
- Node 类型描述 Node API。
- 在前端 tsconfig 中加入 Node 类型，不等于浏览器能使用 fs、process 等任意 Node 能力。
- 不应为了“先消灭报错”给所有模块写返回 any 的空白声明。

### 8.4 模块作用域与重复声明

两个普通脚本文件里的顶层声明可能共享全局作用域，从而出现重复变量错误。含 import/export 的文件通常具有模块作用域，配置也可能影响模块检测。

```ts
export {}; // 可将一个独立练习文件明确标记为模块
```

不要把不同章节中同名的 User、Task 等定义全部堆进一个全局脚本。本文示例独立，是为了让每段更容易阅读。

### 8.5 class、implements 与访问修饰符

```ts
interface Printable {
  print(): string;
}

class UserCard implements Printable {
  readonly id: string;
  private name: string;

  constructor(id: string, name: string) {
    this.id = id;
    this.name = name;
  }

  print(): string {
    return `${this.id}: ${this.name}`;
  }
}

const card = new UserCard('u1', '小轩');
console.log(card.print());
// console.log(card.name); // TS 类型检查不允许
```

- implements 检查类是否满足接口结构，不会自动生成接口中的逻辑。
- public 是常见默认访问级别。
- private、protected 主要参与 TS 的访问与兼容性检查。
- JS 的 `#privateField` 具有运行时私有字段语义，与 TS private 关键字不能完全等同。
- 无论哪种前端私有字段，都不应当成保存服务端密钥的安全方案；代码和运行环境属于客户端。

参数属性简写：

```ts
class Task {
  constructor(public title: string, public done = false) {}
}
```

这会生成实例属性，不只是删除类型注解。某些仅支持类型擦除的直接执行方式不支持所有此类语法，需要看工具链。

Vue 常见组合式写法以对象和函数为主，不要求把每一份接口响应都 new 成一个 class。

### 8.6 enum 与字面量联合

```ts
enum Role {
  User = 'user',
  Admin = 'admin'
}

const role: Role = Role.User;
console.log(role);
```

普通 enum 通常生成运行时对象。数字枚举还涉及反向映射等额外行为，初学不要机械照搬 Java enum 的所有理解。

轻量状态也可以这样写：

```ts
const Roles = {
  User: 'user',
  Admin: 'admin'
} as const;

type Role = typeof Roles[keyof typeof Roles];
const role: Role = Roles.User;
```

若不需要运行时的集中常量对象，直接 `type Role = 'user' | 'admin'` 也很清晰。选择应结合团队约定、运行时需求与工具链，不必把枚举当成每个状态的必选项。

---

## 第9章 DOM、接口请求与运行时校验

### 9.1 DOM 为什么经常提示可能为 null

```ts
const input = document.querySelector('#username');
// input.value; // 可能为 null，而且 Element 未必有 value

if (input instanceof HTMLInputElement) {
  console.log(input.value.trim());
}
```

这是两个问题：可能没找到元素，找到的也不一定是输入框。instanceof 同时解决这两个运行时判断。

```ts
const input = document.querySelector<HTMLInputElement>('#username');
console.log(input?.value);
```

带泛型的 querySelector 可以表达调用方预期的元素类型，但不会验证选择器实际匹配结果一定是 input。静态类型中的 HTMLInputElement 与页面真实结构仍要一致。

### 9.2 事件对象与 target/currentTarget

```ts
function handleInput(event: Event): void {
  const target = event.target;
  if (target instanceof HTMLInputElement) {
    console.log(target.value);
  }
}
```

event.target 的类型范围较宽，因为一个事件不保证来自某种特定元素。currentTarget 表示当前监听器关联的目标，但 TS 的具体类型也需要结合监听方式处理。

不要为消除错误无条件写 `(event.target as any).value`。

Vue 中将这个函数绑定到输入事件即可使用，具体写法见下一章。

### 9.3 给 fetch 返回值标类型的常见误区

```ts
interface User { id: string; name: string; }

async function loadUserUnsafe(): Promise<User> {
  const response = await fetch('/api/users/u1');
  return response.json();
}
```

上面的返回类型可能让调用方得到 User 提示，但既没有检查 HTTP 状态，也没有验证 JSON 结构。常见 DOM 声明中 response.json() 的类型本身也可能是 any，因此不能把“编译不报错”当作接口正确证明。

泛型请求库同理：写 `request<User>(...)` 只是表达期待，不会凭空产生 User 验证器。

### 9.4 一个带解码函数的 JSON 请求封装

```ts
interface User {
  id: string;
  name: string;
}

type Decoder<T> = (value: unknown) => T;

function decodeUser(value: unknown): User {
  if (typeof value !== 'object' || value === null || Array.isArray(value)) {
    throw new Error('用户响应不是对象');
  }

  const record = value as Record<string, unknown>;
  if (typeof record.id !== 'string' || typeof record.name !== 'string') {
    throw new Error('用户响应字段不合法');
  }

  return { id: record.id, name: record.name };
}

async function requestJson<T>(
  url: string,
  decode: Decoder<T>,
  init?: RequestInit
): Promise<T> {
  const response = await fetch(url, init);
  if (!response.ok) {
    throw new Error(`请求失败：HTTP ${response.status}`);
  }
  const raw: unknown = await response.json();
  return decode(raw);
}

async function loadUser(): Promise<User> {
  return requestJson('/api/users/u1', decodeUser);
}
```

这个帮助函数明确约定成功响应必须有 JSON，不能直接用于 204 无响应体、文件下载、文本等接口。空响应或非法 JSON 会在解析时失败，调用方要处理错误。

decodeUser 中的 Record 断言只用于把已判断为非空对象的值当作未知属性集合访问，具体 User 字段仍实际检查，之后重新构造需要的数据。

这里的 T 与 decode 关联，因此通常能从 decodeUser 推断出 User，不必每次显式写 `<User>`。

### 9.5 通用响应包装与分页

假设后端契约明确为：成功时 `success: true`，失败时 `success: false`。可这样建模：

```ts
type ApiResult<T> =
  | { success: true; data: T }
  | { success: false; message: string };

interface PageResult<T> {
  items: T[];
  total: number;
}

interface User { id: string; name: string; }

function getItems(result: ApiResult<PageResult<User>>): User[] {
  if (!result.success) throw new Error(result.message);
  return result.data.items;
}
```

这段函数处理的是已经可信地符合契约的数据。若它来自网络，仍要在边界验证 success 和对应分支字段。

如果 Java 后端采用 `{ code, message, data }`，就按那个结构定义，不要为了套用示例而虚构后端字段。Java 实体类、请求 DTO 和前端展示模型也不一定完全相同。

### 9.6 catch 变量按 unknown 处理

```ts
function errorMessage(error: unknown): string {
  if (error instanceof Error) return error.message;
  if (typeof error === 'string') return error;
  return '发生未知错误';
}
```

JS 可以 throw 字符串、对象等任意值，所以不能无条件认定所有 catch 到的值都是 Error。网络层、页面层可以统一组织错误展示，但不应吞掉所有失败后假装返回正确结果。

### 9.7 DTO、日期与前后端类型映射

| 后端概念 | 前端常见表示 | 注意 |
| --- | --- | --- |
| long 类型 ID | string | 避免超出 JS 安全整数范围 |
| LocalDateTime/Instant 等 | 明确约定格式的 string | 时区和解析规则要一致 |
| BigDecimal 金额 | 明确契约的 string 或最小单位整数等 | 不要只改成 number 就期待精确 |
| Java enum | 字符串或数字字面量联合等 | 以实际序列化结果为准 |
| 可为空字段 | T \| null 或可选属性 | 缺失、null、空字符串含义可能不同 |
| 分页对象 | PageResult<T> 等 | 字段名与数据层级必须对应 |

仅仅把返回类型写成 Date，不会把 JSON 里的日期字符串变成 Date 实例。需要运行时转换，并处理无效日期。

---

## 第10章 Vue 3 中最常用的 TypeScript 写法

### 10.1 先建立使用边界

本章示例需要 Vue 3 工程，使用组合式 API 和 `<script setup lang="ts">`。

只给 script 加 lang="ts" 不会自动为所有数据补上正确类型，也不会把所有运行时错误变成编译错误。先用类型描述状态、参数和事件，再让 Vue 建立状态与界面的联系。

### 10.2 ref：状态类型与初始值

```ts
import { ref } from 'vue';

interface User { id: string; name: string; }
interface Task { id: string; title: string; done: boolean; }

const count = ref(0); // 通常可推断，无需 ref<number>(0)
const user = ref<User | null>(null);
const tasks = ref<Task[]>([]);

count.value += 1;
user.value = { id: 'u1', name: '小轩' };
tasks.value.push({ id: 't1', title: '学习 TS', done: false });
```

尤其关注两个常见初始值：

- 当前无值：用 `ref<User | null>(null)` 表达未来会加载 User。
- 空数组：用 `ref<Task[]>([])` 表达之后的元素结构，避免推断不符合预期。

JS 中访问 ref 通常需要 .value；模板中顶层 ref 有自动解包规则，不能把模板习惯直接带入 JS。

### 10.3 reactive 与 computed

```ts
import { reactive, computed } from 'vue';

interface LoginForm {
  username: string;
  password: string;
}

const form: LoginForm = reactive({ username: '', password: '' });
const canSubmit = computed(() =>
  form.username.trim().length > 0 && form.password.length >= 8
);
```

简单对象优先利用推断或清晰的变量类型标注。reactive 对嵌套 ref 有解包规则，复杂场景不要机械把传入的原始形状当作返回类型，也不要为消除报错滥用泛型或断言。

computed 常能自动推断结果。它用于派生数据，不应在普通计算回调里混杂请求、修改其他状态等副作用。

### 10.4 props 的类型声明与默认值

在 `<script setup lang="ts">` 内：

```ts
interface Props {
  title: string;
  disabled?: boolean;
  tags?: string[];
}

const props = withDefaults(defineProps<Props>(), {
  disabled: false,
  tags: () => []
});
```

defineProps、withDefaults 是 script setup 编译宏，在相应上下文中使用，不需要按普通运行时函数从 Vue 导入。

使用 withDefaults 时，数组和对象默认值用工厂函数，避免实例之间共享可变默认值。Vue 3.5 及之后的响应式 props 解构提供另一种写默认值的方式；旧课程与旧版本行为可能不同，本笔记先采用上面易于对应多个版本的形式。

不要同时在同一次 defineProps 调用中混用泛型类型声明和运行时对象声明。

### 10.5 emits：明确事件名与参数

```ts
const emit = defineEmits<{
  (event: 'select', id: string): void;
  (event: 'save', title: string, done: boolean): void;
}>();

emit('select', 't1');
emit('save', '学习 TS', false);
// emit('select', 123); // 类型错误
```

Vue 3.3+ 也可使用事件名映射到参数元组的简写：

```ts
const emit = defineEmits<{
  select: [id: string];
  save: [title: string, done: boolean];
}>();
```

这些写法为组件调用建立静态契约，不意味着所有外部输入都完成了业务验证。

### 10.6 DOM 模板引用与原生事件

```ts
import { ref, onMounted } from 'vue';

const inputRef = ref<HTMLInputElement | null>(null);
onMounted(() => inputRef.value?.focus());

function handleInput(event: Event): void {
  if (event.target instanceof HTMLInputElement) {
    console.log(event.target.value);
  }
}
```

在模板中对应 `<input ref="inputRef" @input="handleInput">`。

引用可能在挂载前、卸载后或条件渲染不展示时为空，因此类型需要表达 null。不要只用非空断言强行忽略生命周期。

### 10.7 一个完整的输入组件示例

保存为 Vue 工程中的 `TaskInput.vue`：

```vue
<script setup lang="ts">
import { computed, onMounted, ref } from 'vue';

interface Props {
  initialTitle?: string;
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  initialTitle: '',
  disabled: false
});

const emit = defineEmits<{
  (event: 'submit-title', title: string): void;
}>();

const draft = ref(props.initialTitle);
const inputRef = ref<HTMLInputElement | null>(null);
const canSubmit = computed(() =>
  !props.disabled && draft.value.trim().length > 0
);

function submit(): void {
  const title = draft.value.trim();
  if (!canSubmit.value || title.length > 100) return;
  emit('submit-title', title);
  draft.value = '';
  inputRef.value?.focus();
}

onMounted(() => inputRef.value?.focus());
</script>

<template>
  <form @submit.prevent="submit">
    <label>
      新任务
      <input
        ref="inputRef"
        v-model="draft"
        :disabled="props.disabled"
        maxlength="100"
        required
      >
    </label>
    <button type="submit" :disabled="!canSubmit">添加</button>
  </form>
</template>
```

这个例子串联了 ref、computed、props、emits、表单、DOM 引用和函数类型。

initialTitle 只用于初始化 draft，不会因为父组件后续修改 initialTitle 就自动同步 draft；如果业务需要持续同步，应再设计 watch、v-model 或其他明确的数据流，不能把初始化赋值当作响应式绑定。

父组件接收事件的核心写法是 `@submit-title="handleCreate"`，并定义 `function handleCreate(title: string): void`。

### 10.8 Vue 项目中怎么检查类型

在已安装 vue-tsc、TypeScript 且配置匹配的 Vue 工程中，常见检查命令是：

```bash
npx vue-tsc --noEmit
```

使用项目引用的工程，脚手架可能提供 `vue-tsc -b` 或专门的 type-check 脚本；优先运行实际 package.json 中的约定。本文不要求你脱离工程配置照搬单一命令。

仅仅页面能在 dev server 打开，不能证明所有 TS 和模板类型检查都已通过。类型检查工具与构建工具各自承担的工作要分清。

官方参考：[Vue 与 TypeScript](https://vuejs.org/guide/typescript/overview.html)、[组合式 API 类型用法](https://vuejs.org/guide/typescript/composition-api.html)。

---

## 第11章 综合练习：带类型与校验的任务数据模块

### 11.1 练习目标

本例把任务数据处理从界面中抽出来，之后可接到 Vue 中。它不需要浏览器、不请求后端，运行输出使用 console。

需要三个源码文件和第2章的 tsconfig.json：

| 文件 | 职责 |
| --- | --- |
| src/types.ts | 任务结构、创建参数、更新参数 |
| src/tasks.ts | 校验、创建、更新、筛选、状态文案 |
| src/main.ts | 运行演示 |
| tsconfig.json | 编译配置 |

### 11.2 src/types.ts

```ts
export type TaskStatus = 'todo' | 'done';

export interface Task {
  readonly id: string;
  title: string;
  status: TaskStatus;
}

export type CreateTaskInput = Pick<Task, 'title'>;
export type UpdateTaskInput = Partial<Pick<Task, 'title' | 'status'>>;
```

### 11.3 src/tasks.ts

```ts
import type { CreateTaskInput, Task, TaskStatus, UpdateTaskInput } from './types.js';

export function parseTask(value: unknown): Task {
  if (typeof value !== 'object' || value === null || Array.isArray(value)) {
    throw new Error('任务必须是对象');
  }

  const record = value as Record<string, unknown>;
  if (typeof record.id !== 'string' || record.id.trim() === '') {
    throw new Error('任务 ID 不合法');
  }
  if (typeof record.title !== 'string') {
    throw new Error('任务标题必须是字符串');
  }
  const title = record.title.trim();
  if (title.length === 0 || title.length > 100) {
    throw new Error('任务标题需为 1～100 个字符');
  }
  if (record.status !== 'todo' && record.status !== 'done') {
    throw new Error('任务状态不合法');
  }

  return { id: record.id, title, status: record.status };
}

export function parseTasks(json: string): Task[] {
  const value: unknown = JSON.parse(json);
  if (!Array.isArray(value)) throw new Error('任务集合必须是数组');
  const tasks = value.map(item => parseTask(item));
  if (new Set(tasks.map(task => task.id)).size !== tasks.length) {
    throw new Error('任务 ID 不能重复');
  }
  return tasks;
}

export function createTask(id: string, input: CreateTaskInput): Task {
  return parseTask({ id, title: input.title, status: 'todo' });
}

export function updateTask(task: Task, patch: UpdateTaskInput): Task {
  return parseTask({
    id: task.id,
    title: patch.title ?? task.title,
    status: patch.status ?? task.status
  });
}

export function filterTasks(
  tasks: readonly Task[],
  status?: TaskStatus
): Task[] {
  return tasks.filter(task => status === undefined || task.status === status);
}

function assertNever(value: never): never {
  throw new Error(`未知状态：${String(value)}`);
}

export function statusLabel(status: TaskStatus): string {
  switch (status) {
    case 'todo': return '待办';
    case 'done': return '完成';
    default: return assertNever(status);
  }
}
```

这里 filterTasks 接受只读数组，表明函数不应修改输入数组结构；它返回新数组，但里面的 Task 对象仍可能与输入共享，并不是深拷贝。

updateTask 的 patch 是由程序内已符合契约的参数产生；如果 patch 本身来自任意网络 JSON，还要先验证 patch，不能把这个类型注解当作外部数据防线。

本例的长度采用 JS string.length，按 UTF-16 码元计数，不严格等于用户视觉字符数量。

### 11.4 src/main.ts

```ts
import type { Task } from './types.js';
import { createTask, filterTasks, parseTasks, statusLabel, updateTask } from './tasks.js';

const tasks: Task[] = [
  createTask('t1', { title: ' 学习 interface ' }),
  createTask('t2', { title: '理解泛型' })
];

const updated = tasks.map(task =>
  task.id === 't1' ? updateTask(task, { status: 'done' }) : task
);

console.table(updated.map(task => ({
  id: task.id,
  title: task.title,
  status: statusLabel(task.status)
})));

console.log('未完成数量：', filterTasks(updated, 'todo').length);

const json = JSON.stringify(updated);
const restored = parseTasks(json);
console.log('恢复数量：', restored.length);

try {
  parseTasks('[{"id":"bad","title":"","status":"todo"}]');
} catch (error: unknown) {
  console.log(error instanceof Error ? error.message : '未知错误');
}
```

### 11.5 编译与运行

在独立练习目录安装 TypeScript，使用第2章的 tsconfig.json：

```bash
npm init -y
npm pkg set type=module
npm install --save-dev typescript
npx tsc -p tsconfig.json
node dist/main.js
```

type=module 让 Node 将输出的 .js 按 ESM 解释，配合源码中明确的 .js 相对导入路径。

预期现象：

- 第一项状态显示“完成”，第二项显示“待办”。
- 未完成数量为 1，恢复数量为 2。
- 空白标题被运行时校验拒绝。
- 创建时会 trim 标题。
- 修改状态没有改变原任务的 id。

### 11.6 自己完成的扩展

1. 新增 doing 状态，观察状态文案、校验和联合类型哪些地方需要同步修改。
2. 添加按关键词筛选，参数仍接受 readonly Task[]。
3. 设计编辑表单专用类型，不允许通过更新参数修改 id。
4. 给网络返回的任务数组添加加载状态，使用可辨识联合。
5. 把模块接入 Vue，在 ref<Task[]> 中管理结果。

更进一步可从单一状态常量派生联合类型和运行时检查，减少状态列表重复维护，但先把显式版本写清楚。

---

## 第12章 学习清单、常见错误与复习

### 12.1 学 Vue 前先掌握到哪里

| 优先级 | 内容 | 目标 |
| --- | --- | --- |
| 先熟练 | 类型注解与推断、数组与对象类型 | 能描述列表数据和表单 |
| 先熟练 | interface/type、可选属性、联合类型 | 能表达接口契约与可空状态 |
| 先熟练 | 函数参数与返回值、Promise<T> | 能给业务函数和请求函数标注类型 |
| 先熟练 | 泛型基础、PageResult<T> | 能读懂分页和通用响应 |
| 先熟练 | 收窄、unknown、as 的边界 | 能解决空值与未知类型问题 |
| Vue 中边用边学 | ref<T>、props、emits、模板引用 | 能写常见 TS 组件 |
| 项目中继续熟悉 | Pick/Omit/Partial/Record 等 | 能派生清晰的输入输出类型 |
| 先认识即可 | 类型映射、条件类型、infer、复杂重载 | 能定位概念，不要求立即熟练设计 |
| 按需专题深入 | 复杂声明文件、装饰器、品牌类型、泛型组件等 | 有具体需求再学 |

对于你的方向，第一阶段重点是能完成 Vue 与 Java 后端的数据对接，不需要先写一套通用类型框架。

### 12.2 常见报错怎么处理

| 报错现象 | 优先排查 | 不建议的第一反应 |
| --- | --- | --- |
| 类型 string 不能赋给 number | 数据是否需要真实转换、后端契约是否一致 | 直接 as number |
| 可能为 null/undefined | 是否是真实业务状态，使用前如何分支处理 | 到处加 ! |
| Property 不存在 | 对象类型是否定义错、层级是否读错 | 改成 any |
| Type never 相关错误 | 空数组推断、冲突交叉类型、已穷尽分支 | 盲目断言成目标类型 |
| 事件 target 没有 value | 是否需要 instanceof 判断具体元素 | target as any |
| 类型必须用 type-only import | 项目模块配置与 import type | 关掉全部严格配置 |
| 模块找不到 | 路径、扩展名、依赖、声明、解析方式 | 随便 declare module 为 any |
| 明明编译通过但接口崩溃 | 外部数据验证与 any/断言传播 | 以为 TS 没有用 |
| 重复声明变量/接口混乱 | 是否把独立示例放进全局作用域 | 随机改名字不理解作用域 |

### 12.3 重要认知速查

| 容易误解 | 正确认识 |
| --- | --- |
| TS 会把 JS 变成 Java | JS 的运行时语义依然存在 |
| User 类型能检查 JSON | 需要实际运行的验证代码 |
| as number 会把字符串转数字 | 它不会执行 Number 转换 |
| value! 保证永远不为空 | 只是告诉编译器忽略此处空值可能 |
| readonly 就完全不可变 | 主要是静态、浅层的访问约束 |
| Partial<T> 会自动补默认值 | 它只构造类型 |
| Omit 会删除对象字段 | 删除/筛选字段要写运行时代码 |
| 接口必须 new 或 implements 才能用 | 普通对象结构满足就可以 |
| 泛型越多越高级 | 应该表达必要的类型关系 |
| 类型检查通过说明请求成功 | HTTP 状态与业务响应仍需处理 |
| dev server 正常就没有类型错误 | 转译与类型检查是不同工作 |
| 必须先学完 TS 才能学 Vue | 掌握核心后可边写 Vue 边深化 |

### 12.4 复习题与参考要点

**1. any 和 unknown 的核心区别是什么？**

参考：any 大幅放松检查，容易让问题继续传播；unknown 要先收窄或验证再操作。

**2. interface 与 type 应该如何选择？**

参考：都能描述对象；type 可表达联合等更多形式，interface 有声明合并等能力，按需求与团队约定选择。

**3. 为什么 UserId = string 不能自动区分用户 ID 和订单 ID？**

参考：普通类型别名不是独立名义类型，仍与 string 兼容。

**4. 为什么 Task[] 和 readonly Task[] 不等于深层可变与深层不可变？**

参考：数组结构的写入权限与内部对象属性的可变性是不同层次。

**5. 什么情况下需要 T | undefined？**

参考：真实结果可能不存在，例如空数组第一项、查找未命中、字典键缺失等。

**6. K extends keyof T 与 T[K] 分别做什么？**

参考：限定属性键合法，并提取该键对应的值类型，建立输入输出关系。

**7. 为什么 as User 无法替代 decodeUser？**

参考：前者没有运行时验证，后者执行实际字段检查并构造结果。

**8. 为什么类型为 Partial<User> 的更新参数可能太宽？**

参考：把不应允许修改的字段也纳入了契约，应 Pick/Omit 或单独定义输入模型。

**9. Vue 空数组状态为什么经常写 ref<Task[]>([])？**

参考：明确未来元素类型，避免单凭空数组推断得到不合预期的类型。

**10. TS 的类型检查与后端参数校验能互相替代吗？**

参考：不能。客户端能被绕过，后端需独立验证；前端对不可信响应也应设计数据边界。

### 12.5 官方资料与后续查阅

以下资料用于核对规则、查看详细限制和版本差异。本笔记中的业务模型和练习是为学习重新组织的示例，不代表所有项目都必须采用同样的字段或封装。

| 主题 | 官方文档 |
| --- | --- |
| TypeScript 总览 | [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) |
| 日常类型 | [Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) |
| 收窄与控制流 | [Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html) |
| 对象类型 | [Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html) |
| 泛型 | [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) |
| 工具类型 | [Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html) |
| tsconfig | [What is a tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) |
| satisfies 的引入与用途 | [TypeScript 4.9](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html) |
| Vue 工程与 TS | [Using Vue with TypeScript](https://vuejs.org/guide/typescript/overview.html) |
| Vue 组合式 API 类型 | [TypeScript with Composition API](https://vuejs.org/guide/typescript/composition-api.html) |

建议先完成第1～6章，理解第7章常用工具类型，再结合 Vue 课程使用第10章。第11章可以作为阶段练习，第8章和进阶类型内容按项目需要回查。
