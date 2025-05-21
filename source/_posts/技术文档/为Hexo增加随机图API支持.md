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
hexo.extend.generator.register('post', locals => {
  const previousIndexes = []

  const getRandomCover = defaultCover => {
    if (!defaultCover) return false
    if (!Array.isArray(defaultCover)) return defaultCover

    const coverCount = defaultCover.length

    if (coverCount === 1) {
      return defaultCover[0]
    }

    const maxPreviousIndexes = coverCount === 2 ? 1 : (coverCount === 3 ? 2 : 3)

    let index
    do {
      index = Math.floor(Math.random() * coverCount)
    } while (previousIndexes.includes(index) && previousIndexes.length < coverCount)

    previousIndexes.push(index)
    if (previousIndexes.length > maxPreviousIndexes) {
      previousIndexes.shift()
    }

    return defaultCover[index]
  }

  const handleImg = data => {
    const imgTestReg = /\.(png|jpe?g|gif|svg|webp|avif)(\?.*)?$/i
    let { cover: coverVal, top_img: topImg } = data

    // Add path to top_img and cover if post_asset_folder is enabled
    if (hexo.config.post_asset_folder) {
      if (topImg && topImg.indexOf('/') === -1 && imgTestReg.test(topImg)) {
        data.top_img = `${data.path}${topImg}`
      }
      if (coverVal && coverVal.indexOf('/') === -1 && imgTestReg.test(coverVal)) {
        data.cover = `${data.path}${coverVal}`
      }
    }

    if (coverVal === false) return data

    // If cover is not set, use random cover
    if (!coverVal) {
      const { cover: { default_cover: defaultCover } } = hexo.theme.config
      const randomCover = getRandomCover(defaultCover)
      data.cover = randomCover
      coverVal = randomCover // update coverVal
    }

    if (coverVal && (coverVal.indexOf('//') !== -1 || imgTestReg.test(coverVal))) {
      data.cover_type = 'img'
    }

    // 修改后的SPM参数逻辑：从 cover 配置中读取 spm 参数
    const coverConfig = hexo.theme.config.cover || {} // 从 cover 配置读取
    const mode = coverConfig.spm || 0 // 获取 spm 模式，默认为 0

    if (mode !== 0 && coverVal) {
      const randomNum = Date.now().toString(36) + Math.random().toString(36).slice(2)
      if (mode === 1) {
        data.cover = `${coverVal}?spm=${randomNum}`
      } else if (mode === 2) {
        data.cover = `${coverVal}&spm=${randomNum}`
      }
    }

    return data
  }

  const posts = locals.posts.sort('date').toArray()
  const { length } = posts

  return posts.map((post, i) => {
    if (i) post.prev = posts[i - 1]
    if (i < length - 1) post.next = posts[i + 1]

    post.__post = true

    return {
      data: handleImg(post),
      layout: 'post',
      path: post.path
    }
  })
})
```

打开butterfly主题配置文件：在 cover: 插入 spm: 1 并保存（目的是在链接后面加入后缀 ?spm={随机数} 0 是不使用后缀；1 是？加随机数；2 是 & 加随机数）
