---
title: Jest框架基本知识总结
date: 2020-03-10 20:51:28
cover: http://f.dooomi.com/image/qj9128200803.jpg
---

### 1、安装命令

```
npm install jest --dev or yarn add jest --dev
```

### 2、jest 常用命令

```
jest --init/watch/watchAll/coverage/ci/config
```

### 3、jest 配置

```
// jest.config.js
module.exports = {
  browser: false, //node端使用
  clearMocks: true, //清除Mock信息
  coverageDirectory: "reports", //覆盖率报告目录
  ...
}
```

### 4、jest 全局方法

```
afterAll(fn, timeout)
afterEach(fn, timeout)
beforeAll(fn, timeout)
beforeEach(fn, timeout)
describe(name, fn)
describe.each(table)(name, fn, timeout)
describe.only(name, fn)
describe.only.each(table)(name, fn)
describe.skip(name, fn)
describe.skip.each(table)(name, fn)
test(name, fn, timeout)
test.each(table)(name, fn, timeout)
test.only(name, fn, timeout)
test.only.each(table)(name, fn)
test.skip(name, fn)
test.skip.each(table)(name, fn)
test.todo(name)
```

### 5、jest 匹配器

```
expect(value)
expect.extend(matchers)
expect.anything()
expect.any(constructor)
expect.arrayContaining(array)
expect.assertions(number)
expect.hasAssertions()
expect.not.arrayContaining(array)
expect.not.objectContaining(object)
expect.not.stringContaining(string)
expect.not.stringMatching(string | regexp)
expect.objectContaining(object)
expect.stringContaining(string)
expect.stringMatching(string | regexp)
expect.addSnapshotSerializer(serializer)
.not
.resolves
.rejects
.toBe(value)
.toHaveBeenCalled()
.toHaveBeenCalledTimes(number)
.toHaveBeenCalledWith(arg1, arg2, ...)
.toHaveBeenLastCalledWith(arg1, arg2, ...)
.toHaveBeenNthCalledWith(nthCall, arg1, arg2, ....)
.toHaveReturned()
.toHaveReturnedTimes(number)
.toHaveReturnedWith(value)
.toHaveLastReturnedWith(value)
.toHaveNthReturnedWith(nthCall, value)
.toHaveLength(number)
.toHaveProperty(keyPath, value?)
.toBeCloseTo(number, numDigits?)
.toBeDefined()
.toBeFalsy()
.toBeGreaterThan(number | bigint)
.toBeGreaterThanOrEqual(number | bigint)
.toBeLessThan(number | bigint)
.toBeLessThanOrEqual(number | bigint)
.toBeInstanceOf(Class)
.toBeNull()
.toBeTruthy()
.toBeUndefined()
.toBeNaN()
.toContain(item)
.toContainEqual(item)
.toEqual(value)
.toMatch(regexpOrString)
.toMatchObject(object)
.toMatchSnapshot(propertyMatchers?, hint?)
.toMatchInlineSnapshot(propertyMatchers?, inlineSnapshot)
.toStrictEqual(value)
.toThrow(error?)
.toThrowErrorMatchingSnapshot(hint?)
.toThrowErrorMatchingInlineSnapshot(inlineSnapshot)
```

### 6、测试分组

```
describe('des1', () => {
  test('test1', () => {
  });

  test('test2', () => {
  });

  describe('des2', () => {
    test('test3', () => {
    });
  });
});
```

### 7、异步代码测试

- callback 的测试  
  在 test 第二个参数的函数中传递一个 done 方法参数，然后在回调函数中执行 done()

  ```
  test('测试名称', (done) => {
  getData(url, (data) => {
    expect(data).toEqual({
      success: true
    })
    done()
  })
  })
  ```

- promise 的测试

  1）resolves rejects

  ```
  test('测试名称', () => {
    return expect(myAxios(url)).resolves.toEqual('ok');
  });

  // 处理错误:
  test('测试名称', () => {
    return expect(myAxios(url)).rejects.toEqual('error');
  });
  ```

  2）return promise.then().catch()

  ```
  test('测试名称', async () => {
    return myAxios(url).then((res) => {
      expect(res).toEqual('ok')
    })
  })

  // 处理错误:
  test('测试名称', async () => {
    expect.assertions(1)
    return myAxios(url).catch((res) => {
      expect(res.status).toBe('404')
    })
  })
  ```

  3）async await

  ```
  test('测试名称', async () => {
    const res = await myAxios(url)
    expect(res).toEqual({
      success: 1
    })
  })

  // or
  test('测试名称', async () => {
    const res = await myAxios(url)
    expect(res).toEqual({
      success: 1
    })
  })
  ```

  4）async await 配合 resolves rejects

  ```
  test('测试名称', async () => {
    await expect(myAxios(url)).resolves.toEqual({
      success: 1
    })
  })

  test('测试名称', async () => {
    await expect(myAxios(url)).rejects.toBe(404)
  })
  ```

  5）async await 配合 try...catch 处理错误

  ```
  test('测试名称', async () => {
    expect.assertions(1)
    try {
      await myAxios(url)
    } catch (e) {
      expect(e).toBe(404)
    }
  })
  ```
