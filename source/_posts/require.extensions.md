---
title: require.extensions
categories: node
tags: require.extensions
---

- 作用：自定义模块后缀

- 遗憾的是官方提示弃用了这个属性，不过值得庆幸的是，这个属性永远不会被移除，只是不推荐使用而已。

- 使用举例：

  ```js
  require.extensions['.json'] = function(module, filename) {
    var content = fs.readFileSync(filename, 'utf8');
    try {
      module.exports = JSON.parse(content);
    } catch (err) {
      err.message = filename + ': ' + err.message;
      throw err;
    }
  };
  ```