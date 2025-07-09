# 🧠 ​**一、Jest 的核心优势与设计理念**​

1. ​**零配置启动**​
    安装后无需复杂配置即可运行测试（`npm install --save-dev jest`），自动识别 `.test.js` 或 `.spec.js` 文件。
2. ​**并行测试**​
    智能隔离测试文件并行执行，大幅提升大型项目测试速度。
3. ​**内置覆盖率报告**​
    通过 `jest --coverage` 生成代码覆盖率报告，直观展示未覆盖代码。
4. ​**快照测试（Snapshot Testing）​**​
    捕获组件/数据结构输出，自动对比历史快照，快速检测 UI 或逻辑变更。

---

# ⚙️ ​**二、关键 API 详解**​

## ​**1. 测试定义与组织**​

- ​**`describe(name, fn)`**​
    创建测试套件（组），关联多个测试用例

    ：


    ```javascript
    describe('Math模块', () => {
      // 测试用例...
    });
    ```

- ​**`test(name, fn)` 或 `it(name, fn)`**​
    定义单个测试用例：


    ```javascript
    test('1+2=3', () => {
      expect(add(1, 2)).toBe(3);
    })[3,5](@ref)。
    ```

## ​**2. 断言与匹配器（Matchers）​**​

Jest 使用 `expect(value).matcher(expected)` 结构：

| ​**匹配器**​            | ​**用途**​                       | ​**示例**​                                        |
| -------------------- | ------------------------------ | ----------------------------------------------- |
| `toBe(value)`        | 严格相等（`===`）                    | `expect(sum(1,2)).toBe(3)`                      |
| `toEqual(object)`    | 深度递归对象比较                       | `expect(obj).toEqual({id:1})`                   |
| `toContain(item)`    | 检查数组/字符串包含                     | `expect(['a','b']).toContain('a')`              |
| `toMatch(regex)`     | 正则匹配字符串                        | `expect('hello').toMatch(/ll/)`                 |
| `toThrow(error?)`    | 验证函数抛出异常                       | `expect(() => fn()).toThrow('Error')`           |
| `resolves`/`rejects` | 处理异步 Promise 结果                | `await expect(fetchData()).resolves.toBe(data)` |
| `toHaveBeenCalled()` | 验证 Mock 函数被调用（需搭配 `jest.fn()`） | `expect(mockFn).toHaveBeenCalled()`             |

> 💡 更多匹配器：浮点数比较（`toBeCloseTo(0.3)`）、长度检查（`toHaveLength(3)`）等
>
> 。

## ​**3. 模拟（Mock）功能**​

- ​**`jest.fn(impl?)`**​
    创建可追踪调用的模拟函数：


    ```javascript
    const mockFn = jest.fn(x => x + 1);
    mockFn(1);
    expect(mockFn.mock.calls[0][0]).toBe(1); // 检查参数[4,5](@ref)
    ```

- ​**`jest.mock(modulePath, factory?)`**​
    模拟整个模块（如 API 请求）：


    ```javascript
    jest.mock('./api', () => ({ fetchData: jest.fn(() => Promise.resolve({ data: 'mock' })) })[4,5](@ref)。
    ```

## ​**4. 生命周期钩子**​

控制测试环境初始化与清理：

javascript

运行

复制

```javascript
beforeAll(() => initDB());   // 所有测试前执行
afterEach(() => clearCache()); // 每个测试后执行
beforeEach(() => resetState()); // 每个测试前执行[5](@ref)
```

---

# 🛠️ ​**三、实用技巧与最佳实践**​

1. ​**异步测试**​
    使用 `async/await` 或 `resolves/rejects` 避免回调嵌套。
2. ​**快照更新策略**​
    代码变更后运行 `jest --updateSnapshot` 更新快照

    。
3. ​**组件测试（React/Vue）​**​
    搭配 `@testing-library/react` 或 `@vue/test-utils` 进行 DOM 交互测试

    。
4. ​**配置文件 (`jest.config.js`)​**​
    定制测试环境（如 `testEnvironment: 'jsdom'` 模拟浏览器环境）

    。


在这个 tests/dependencies.test.tsx 文件中，主要用到了以下 jest 的 API：

1. describe

- 作用：用于对一组相关的测试用例进行分组。这样可以让测试输出更有层次感，便于阅读和维护。
- 用法示例：

describe('Form.Dependencies', () => { … })

- 解释：这里把所有和“表单依赖”相关的测试都放在一个 describe 里，输出时会有分组标题。

1. it / test

- 作用：定义一个具体的测试用例。it 和 test 是等价的，都是 jest 提供的 API。
- 用法示例：

it('touched', async () => { … })

test('form level', { initialValues: { field_2: 'bamboo' } });

- 解释：每个 it 或 test 代表一个独立的测试场景。可以用来描述行为或功能点。

1. expect

- 作用：断言。用于判断某个值是否符合预期。expect 返回一个断言对象，可以链式调用各种匹配器（如 toBe、toEqual、toBeTruthy 等）。
- 用法示例：

expect(validated).toBeTruthy();

expect(spy).toHaveBeenCalledTimes(1);

- 解释：比如 expect(validated).toBeTruthy() 判断 validated 是否为真，expect(spy).toHaveBeenCalledTimes(1) 判断 spy 这个 mock 函数是否被调用了一次。

1. jest.fn()

- 作用：创建一个 mock 函数（模拟函数）。可以用来追踪函数的调用情况，比如调用次数、参数等。
- 用法示例：

const spy = jest.fn();

- 解释：在测试中经常用来代替真实函数，便于断言和监控。

1. expect(…).toHaveBeenCalledTimes(n)

- 作用：断言 mock 函数被调用的次数。
- 用法示例：

expect(spy).toHaveBeenCalledTimes(2);

- 解释：比如 expect(spy).toHaveBeenCalledTimes(2) 就是判断 spy 这个 mock 函数是否被调用了两次。

1. expect(…).toBeTruthy() / toBeFalsy() / toEqual() / toBe() / toHaveLength()

- 作用：这些都是 jest 的断言方法，用于判断值的真假、相等、长度等。
- 用法示例：

expect(validated).toBeTruthy();

expect(container.querySelectorAll('input')).toHaveLength(2);

- 解释：这些方法可以灵活地断言各种类型的结果。
