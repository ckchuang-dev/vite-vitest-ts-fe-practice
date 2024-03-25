# 07. `Easy` 手寫 Array.fill

## 🔸 題目描述

實作 `fill` 函式，此函式接收四個參數：

- 一個陣列
- 要替換的 `value`
- `start` 索引
- `end` 索引

該函式會從 `start` 到 `end` 索引 (包含 `start` 但不包含 `end`) 來把陣列的元素換成 `value` 。如果未提供 `start` 索引，則應預設為 0。如果未提供 `end` 索引，則剩餘元素會被替換為 `value`。

```javascript
fill([1, 2, 3], '*')
// => ['*', '*', '*']

fill([1, 2], '*', 2, 3)
// => [1, 2]

fill([1, 2, 3, 4, 5], '*', 1, -1)
// => [1, '*', '*', '*', 5]
```

## ✨ 一點心得

昨天參加完 office hour，消化後得到一個靈感是**分析與思路**應該要能依序有這些部分，就當作真的是面試時遇到這個手寫題：

- 問題釐清：看完題目後有沒有想到什麼不清楚的地方，或確認是否需考慮更進階的 edge cases，避免實作後誤會題目需要的輸出而白做工
- 提出測試案例：這一步看狀況可放在這或最後一步，雖然沒仔細研究過 TDD，但這幾天解題下來我習慣先定義測試案例，確認要滿足哪些基本測資，以及需要考慮哪些 edge cases
- 提出思路：實際寫 code 前先簡單口述可能的做法，若比較複雜的題目可能可以搭配 tldraw、FigJam、Miro 等線上電子白板或 flow 工具表達
- coding：最後一步才是實際將思路寫成 code，如此也可以讓面試官知道你不是個在還沒釐清需求就魯莽開發的人

但儘管這樣說，個人覺得前三步如果在面試中還是盡可能簡潔扼要，自己當面試官時有遇過面試者講了快十五分鐘繞好幾圈都還沒開始做，雖然越聽越亂但只好打斷他「兄弟，你講的我大概懂，不如就直接 coding 吧」

今天來用一題簡單的練練這個流程。

## 💭 分析與思路

### 問題釐清

- 理解這題是想實作 ES6 中的 Array.fill 這個原生方法嗎？
- 這個方法期待要直接去改變原陣列並回傳？
- value 需要考慮各種型別嗎？若 value 能傳入 object 是否用同一個物件即可或是需要深拷貝？
- 需要處理 start、end 傳入不為 number 的狀況嗎？
- start、end 可為負數的話，處理方法是否同範例三或 Array.fill 的定義直接將負值加上 array.length？
- 若 start、end 為負數，且加上 array.length 後仍為負數該如何處理？

### 提出測試案例

根據以上有思考到的邊界條件，用單元測試來表達測試案例：

```ts
describe('Array.fill', () => {
  it('should handle basic cases', () => {
    expect(fill([1, 2, 3], '*')).toEqual(['*', '*', '*']);
    expect(fill([1, 2], '*', 2, 3)).toEqual([1, 2]);
    expect(fill([1, 2, 3, 4, 5], '*', 1, -1)).toEqual([1, '*', '*', '*', 5]);
  });

  it('should return the original array', () => {
    const arr = [1, 2, 3];

    expect(fill(arr, '*')).toBe(arr);
  });

  it('should handle edge cases', () => {
    expect(fill([], '*')).toEqual([]);
    expect(fill([1, 2, 3], '*', 0, 0)).toEqual([1, 2, 3]);
    expect(fill([1, 2, 3], '*', 1, 1)).toEqual([1, 2, 3]);
    expect(fill([1, 2, 3], '*', -1, -1)).toEqual([1, 2, 3]);
    expect(fill([1, 2, 3, 4, 5], '*', 3, 1)).toEqual([1, 2, 3, 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', -3)).toEqual([1, 2, '*', '*', '*']);
    expect(fill([1, 2, 3, 4, 5], '*', -3, -2)).toEqual([1, 2, '*', 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', -5, -2)).toEqual(['*', '*', '*', 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', -7, -2)).toEqual(['*', '*', '*', 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', -7, -4)).toEqual(['*', 2, 3, 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', -7, -5)).toEqual([1, 2, 3, 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', NaN, NaN)).toEqual([1, 2, 3, 4, 5]);
    expect(fill([1, 2, 3, 4, 5], '*', -20)).toEqual(['*', '*', '*', '*', '*']);
  });

  it('should handle objects by reference', () => {
    const arr = fill(Array(3), {});
    arr[0].hi = 'hi';

    expect(arr).toEqual([{ hi: 'hi' }, { hi: 'hi' }, { hi: 'hi' }]);
  });
});
```

從前一步的問題釐清可以知道這題能做的處理偏廣，先解釋一下最後預計實作的定義：

- 同 Array.fill 的定義，先用 Array.fill 跑過一遍確認測試案例都正確通過
- 當 end 超過 array 長度時，設為 array 長度
- 當 array 為空時，不處理直接回傳 array
- 當 start、end 為負數時，加上 array.length 處理
- 當 start、end 為負數時，且加上 array.length 處理後仍為負數，則設為 0
- 不在範圍內或索引值無效依照迭代索引值自動處理範圍
- 若 value 為 object，以同一個位址的物件替換陣列中的值
- 回傳的陣列位址需與原陣列為同一個

### 提出思路

- 先對輸入定型別：
  - `array` 可為任意型別的陣列
  - `value` 可為任意型別
  - `start` 與 `end` 為 number 並分別有預設值為 0 與 array.length
- 當 start、end 為負數時，分別去加上 array.lenght 處理，若仍為負數則歸 0
- 當 end 超過 array 長度，則定為陣列長度
- 直接對 array 用 start 與 end 迭代，並用 value 直接替代目標值

### 實作

```ts
const fill = <T, V>(array: (T | V)[], value: V, start = 0, end = array.length) => {
  // 避免對原參數操作，另外宣告 state 存 start、end
  const state = {
    start,
    end,
  };

  // start 的負數處理
  if (start < 0) {
    state.start += array.length;

    if (state.start < 0) {
      state.start = 0;
    }
  }

  // end 的負數處理
  if (end < 0) {
    state.end += array.length;

    if (state.end < 0) {
      state.end = 0;
    }
  }

  if (end > array.length) {
    state.end = array.length;
  }

  // 對原陣列迭代並直接以 value 取代目標值
  for (let i = state.start; i < state.end; i++) {
    array[i] = value;
  }

  // 回傳原陣列
  return array;
};
```
