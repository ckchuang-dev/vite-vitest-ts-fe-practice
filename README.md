# 30 天前端程式題挑戰

## 這是什麼？

前陣子在 3 月初參加 ExplainThis 的前端手寫題打卡群，參考 [50 題前端手寫練習題](https://explainthisio.notion.site/ExplainThis-50-8fe7055e22d5467586f7d2c22719684f)這個題組與框架來練習一些題目，並做一些筆記在此 repo 中。

## Tech-stack

瀏覽了 50 題的內容，想說應該還可以掌握，想順便玩玩新東西，而因為寫這種 util 類的題目，之前遇過有公司面試會使用類似 TDD 的方式在 CodeSandbox 先提前準備好 unit test 測資，並希望能完成主程式來讓所有測資 all pass。

因此起了這個 repo，順便玩玩 Vitest 看看寫起來與 jest 有何不同，參考官方文件設定了一番，也順便記錄一下：

```shell
$ pnpm add -g pnpm # 只是想更新一下 pnpm version

$ pnpm create vite
    -> 選 vanilla、TypeScript

$ cd vite-vitest-ts
$ pnpm i
$ pnpm add -D vitest
```

最後將這段放上 `package.json` 就完工了：

```json
"scripts": {
    "test": "vitest"
},
```

接下來就能在之後幾天用以下每個資料夾的結構來練習寫單元測試及完成手寫題，並完成思路筆記。

搭配 `pnpm run test` 可以執行所有的測試，確認主程式是否能通過能正確涵蓋所有測資。

## 解題進度

- Day01 - [29 - `Easy` sleep](src/29-sleep)
- Day02 - [17 - `Easy` Remove Duplicates](src/17-deduplication)
- Day03 - [30 - `Easy` Promise.race](src/30-promise-race)
- Day04 - [01 - `Easy` lodash.clamp](src/01-clamp)
- Day05 - [02 - `Easy` inRange](src/02-inRange)
- Day06 - [31 - `Easy` add promises](src/31-addPromises)
- Day07 - [32 - `Easy` cancelable timeout](src/32-cancelableTimeout)
- Day08 - [33 - `Easy` cancelable interval](src/33-cancelableInterval)
- Day09 - [34 - `Medium` repeat](src/34-repeat)
- Day10 - [05 - `Easy` lodash.dropWhile](src/05-dropWhile)
- Day11 - [03 - `Easy` lodash.compact](src/03-compact)
- Day12 - [04 - `Easy` lodash.difference](src/04-difference)
- Day13 - [35 - `Medium` functions Promise.all](src/35-promiseAll)
- Day14 - [06 - `Easy` lodash.dropRightWhile](src/06-dropRightWhile)
- Day15 - [07 - `Easy` Array.fill](src/07-fill)
- Day16 - [08 - `Easy` lodash.fromPairs](src/08-fromPairs)
- Day17 - [14 - `Easy` lodash.findIndex](src/14-findIndex)
- Day18 - [15 - `Easy` Array.prototype.square](src/15-square)
- Day19 - [16 - `Easy` Array.getAverage](src/16-getAverage)
- Day20 - [18 - `Easy` Array.prototype.at](src/18-arrayPrototypeAt)
- Day21 - [19 - `Easy` Array.prototype.last](src/19-arrayLast)
- Day22 - [25 - `Medium` consolidate array data](src/25-consolidateData)
- Day23 - [20 - `Easy` lodash.chunk](src/20-chunk)
- Day24 - [21 - `Easy` Array.map](src/21-arrayMap)
- Day25 - [36 - `Medium` Promise with time limit](src/36-promiseWithTimeLimit)
- Day26 - [38 - `Easy` Function Composition](src/38-functionComposition)
- Day27 - [22 - `Easy` Array filter](src/22-filter)
- Day28 - [09 - `Medium` lodash.get](src/09-lodashGet)
- Day29 - [47 - `Easy` Calculator class](src/47-calculatorClass)
- Day30 - [44 - `Easy` To Be or Not To Be, expect.toBe](src/44-expectToBe)
