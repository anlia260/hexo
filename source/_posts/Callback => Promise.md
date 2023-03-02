---
title: Callback => Promise
date: 2021-04-01 13:00:09
categories: "实践"
tags:
     - 实践
---


 
在小程序中，微信小程序的语法亦然采用的callback方式。
为了使用顺手，我希望


```ts
// cumbersome 😭
Taro.login({
  success: res => {
    // do something
  },
  fail: err => {
    // do something
  }
});

// concise 😊
import { promisify, asynclogin } from "promisify.ts"
import Taro from "@tarojs/taro";
try {
  const { code } = await promisify(Taro.login)();

  // do something
  
} catch (error) {
  // do something
}
```

<!-- more -->

## 为了达到这个目的需要一个包装器(promisify.ts)

```ts
import Taro from "@tarojs/taro";
/**
 * 将方法promise化
 * 替换['fail', 'success', 'complete']
 * @param method
 */

type IMethodType = (options: any) => void;

const promisify = (method: IMethodType) => {
   if (typeof method !== 'function') {
     throw new Error(`method is not a function.`);
   }
   return (options: object = {}): Promise<any> => {
    return new Promise((resolve, reject) => {
      method(
        Object.assign({}, options, {
          success: resolve,
          fail: reject
        })
      );
    });
  };
};

export const asynclogin = promisify(Taro.login);

export default promisify;

```


# 🔥🔥🔥 Just have a small fun  ! 