---
title: object-fit
categories: css
tags: css object-fit
---

1. 作用：图片以何种形式在被包裹的div中展示
2. 用法注意：图片的宽、高都设置为100%
3. 属性：
   - `fill`：不保持纵横比缩放图片，使图片完全适应
   - `contain`：保持纵横比缩放图片，使图片的长边能完全显示出来
   - `cover`：保持纵横比缩放图片，只保证图片的短边能完全显示出来
   - `none`：保持图片宽高不变
   - `scale-down`：当图片实际宽高小于所设置的图片宽高时，显示效果与`none`一致；否则，显示效果与`contain`一致