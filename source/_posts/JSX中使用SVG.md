---
title: JSX中使用SVG
date: 2017-10-21 21:36:23
categories: "React"
tags:
     - 开发框架
     - React
---

1. webpack 增加 `svg-inline-loader`

  ```
  npm i svg-inline-loader --save-dev

  {
  test: /\.svg$/,
  loader: 'svg-inline-loader'
  }
  ```

2. JSX使用 svg

  ```
  const newad = require('../../public/newad.svg')
  <svg style={{fill: "#fff"}} dangerouslySetInnerHTML={{__html: newad }} />
  ```
