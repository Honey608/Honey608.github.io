---
layout: title
title: Element Plus 源码分析
date: 2026-01-03 15:43:22
categories: web
tags: ["Element Plus 源码分析"]
---
{% cq %} Element Plus 源码分析 {% endcq %}

<!--more-->
### 构建流程
参考资料：

[https://juejin.cn/post/7076941611216666654?from=search-suggest](https://juejin.cn/post/7076941611216666654?from=search-suggest)

[https://juejin.cn/post/7058190805768339470](https://juejin.cn/post/7058190805768339470)

```javascript
"build": "pnpm run -C internal/build start"
```

```javascript
"start": "gulp --require @esbuild-kit/cjs-loader -f gulpfile.ts"
```

### 样式篇
参考资料：

[https://juejin.cn/post/7190370726677839932?from=search-suggest](https://juejin.cn/post/7190370726677839932?from=search-suggest)

[https://juejin.cn/post/7101968262061113357](https://juejin.cn/post/7101968262061113357)

#### 巧妙的封装 BEM
el 通过mixin 实现了 BEM (命名空间+模块 + 元素名 + 修饰器名) `namespace-block__element--modifier

```sass
$namespace: 'el'; // 命名空间
$element-separator: '__'; // 元素分隔符
$modifier-separator: '--'; // 修饰器分隔符
$state-prefix: 'is-' !default;
@mixin b ($block) {
  // !global 将局部变量转为全局变量
  $B: $namespace + '-' + $block !global;
  // 模板语法
  .#{$B} {
    @content;
  }
}
@mixin e($element) {
  $E: $element !global;
  $selector: &; // & 表示父类选择器
  $currentSelector: "";

  @each $unit in $element {
    $currentSelector: #{$currentSelector + '.' + $B + $element-separator + $unit + ','}
  }

  @if hitAllSpecialNestRule($selector) {
    // 如果 包含修饰器 状态 或者 伪类 则
    // 返回 与父类同级
    @at-root {
      #{$selector} {
        #{$currentSelector} {
          @content;
        }
      }
    }
  } @else {
    @at-root {
      #{$currentSelector} {
        @content;
      }
    }
  }
}

@mixin m ($modifier) {
  $selector: &;
  $currentSelector: "";

  @each $unit in $modifier {
    $currentSelector: #{$currentSelector + & + $modifier-separator + $unit + ","};
  }

  @at-root {
    #{$currentSelector} {
      @content;
    }
  }
}

// 判断是否存在特殊嵌套 包含修饰器，包含状态 包含伪类
@function hitAllSpecialNestRule($selector) {
  @return containsModifier($selector) or containWhenFlag($selector) or containPseudoClass($selector);
}

@function selectorToString($selector) {
  // inspect($value) 将列表转换成字符串
  $selector: inspect($selector);
  // str-slice($string, $start-at, $end-at) 截取字符串
  $selector: str-slice($selector, 2, -2);
  @return $selector;
}

// 选择器是否包含修饰符
@function containsModifier($selector) {
  $selector: selectorToString($selector);

  // 检查 选择器中 是否存在修饰器分隔符
  @if str-index($selector, $modifier-separator) {
    @return true;
  } @else {
    @return false;
  }
}

@function containWhenFlag($selector) {
  $selector: selectorToString($selector);
  // 是否包含状态前缀
  @if str-index($selector, '.' + $state-prefix) {
    @return true
  } @else {
    @return false
  }
}

@function containPseudoClass($selector) {
  $selector: selectorToString($selector);

  // 是否包含伪类
  @if str-index($selector, ':') {
    @return true
  } @else {
    @return false
  }
}
.test-bem{
    color: purple;
    @include b(heade1r) {
        color:orange;
    }
    @include e(aa) {
        color:red;
        @include m (success) {
            color: green;
        }
    }
}
// 输出结果
.test-bem {
  color: purple;
}
.test-bem .el-heade1r {
  color: orange;
}
.el-heade1r__aa {
  color: red;
}
.el-heade1r__aa--success {
  color: green;
}
```

#### CSS 变量封装
