---
title: 为Hexo增加随机图API支持
date: 2025-05-17 19:38:59
# updated:
# tags:
categories: 技术文档
# keywords:
# description:
top_img: transparent
# comments:
# cover:
# toc:
# toc_number:
# toc_style_simple:
# copyright:
# copyright_author:
# copyright_author_href:
# copyright_url:
# copyright_info:
# mathjax:
# katex:
# aplayer:
# highlight_shrink:
# aside:
# abcjs:
# noticeOutdate:
---

将根目录下创建`scripts/filters/random_cover.js`文件，内容为

```js
/**
 * Butterfly
 * ramdom cover
 */

'use strict'

hexo.extend.filter.register('before_post_render', function (data) {
  const { config } = this
  if (config.post_asset_folder) {
    const imgTestReg = /\.(png|jpe?g|gif|svg|webp)(\?.*)?$/
    const topImg = data.top_img
    const cover = data.cover
    if (topImg && topImg.indexOf('/') === -1 && imgTestReg.test(topImg)) data.top_img = data.path + topImg
    if (cover && cover.indexOf('/') === -1) data.cover = data.path + cover
  }

  if (data.cover === false) {
    data.randomcover = randomCover()
    return data
  }

  data.cover = data.cover || randomCover()
  return data
},999)// 改成最高优先级，最后执行

function randomCover () {
  const theme = hexo.theme.config
  let cover
  let num

  if (theme.cover && theme.cover.default_cover) {
    if (!Array.isArray(theme.cover.default_cover)) {
      cover = theme.cover.default_cover
    } else {
      num = Math.floor(Math.random() * theme.cover.default_cover.length)
      cover = theme.cover.default_cover[num]
    }
  } else {
    cover = theme.default_top_img || 'data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7'
  }
  if(theme.cover.suffix){
    const randomNum = Date.now().toString(36) + Math.random().toString(36).slice(2)
    if(theme.cover.suffix == 1)
      cover = cover + '?' + randomNum
    else if(theme.cover.suffix == 2)
      cover = cover + '&' + randomNum
  }
  return cover
}

```

打开butterfly主题配置文件：在 cover: 插入suffix: 1 并保存（目的是在链接后面加入后缀 ?随机数 0 是不使用后缀；1 是？加随机数；2 是&加随机数）

---
参考文章

[hexo butterfly主题添加对随机图片api的支持](https://blog.mitsumune.top/2023/02/13/hexo_butterfly%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E5%AF%B9%E9%9A%8F%E6%9C%BA%E5%9B%BE%E7%89%87api%E7%9A%84%E6%94%AF%E6%8C%81/)