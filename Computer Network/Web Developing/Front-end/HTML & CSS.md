> [!info] CodingStartup  
> [https://www.youtube.com/channel/UCwYb4QztAKKqj5YcaTEtyrQ](https://www.youtube.com/channel/UCwYb4QztAKKqj5YcaTEtyrQ)  
# HTML
1. Basic Structure
    ```HTML
    <html>
      <head>
        <!-- metadata and css -->
        <link rel="stylesheet" href="[css files].css">
      </head>
      <body>
        <header></header>
        <main></main>
        <footer></footer>
      </body>
      <script src="[js files].js"></script>
    </html>
    ```
    Quick Generate: ! + return
    Generate Passages: Lorem* <# of words>
2. Forms
```
    <form action="/login" method="post">
      <label for="userid">User ID: </label>
      <input id="your_name" type="text" name="user_id" value="">
      <label for="password">Password: </label>
      <input id="your_name" type="text" name="password" value="">
      <input type="submit" value="Login">
    </form>
```
4. Resources
    1. Documents
        [https://www.w3schools.com/](https://www.w3schools.com/)
        [https://developer.mozilla.org/](https://developer.mozilla.org/)
    2. Resources
        Download in China
        [https://www.bootcdn.cn](https://www.bootcdn.cn/)
        Icon
        [https://www.iconfont.cn](https://www.iconfont.cn/)
        [http://www.fontawesome.com](http://www.fontawesome.com/)
        Sound
        [http://www.aigei.com/](http://www.aigei.com/)
        Dummy Image
        [https://dummyimage.com/](https://dummyimage.com/)
    3. Tools
        Browser: Chrome, Firefox, Safari(Safari devtools⇒layers is convenient for z-index debugging)
        Atom extension: Emmet
        VScode extension: Live Server
        Command line: browser-sync
# CSS
- Core Topics
    - Selector and Pseudo-Selector
        `element` `.class` `#id`
        > alternative selecting method: xpath
    - Display: block, inline-block, inline, none
        Inline: cannot change vertical layout
        block, inline-block: can change vertical layout
        block: put next element below itself
        Inline, inline-block: put next element next to itself
    - Box Model: **Content=>Padding=>Border**(=>Outline)=>Margin
        margin/padding: a (top right bottom left)
        a b (top bottom) (right left)
        a b c (top) (right left) (bottom)
        a b c d (top) (right) (left) (bottom)
        Margin Collapse
        box-sizing: content-box/border-box;
    - Position: static (default), absolute, relative, fixed, sticky
    - Size:
        - px: absolute (be careful of the line-height)
        - em: relative to parent container
        - rem: relative to root container
        - vw: relative to viewport width
        - vh: relative to viewport height
        - vmin: relative to min(viewport.width, viewport.height)
        - vmax: relative to max(viewport.width, viewport.height)
    - Float: (Don't use it unless you want text-around-image effect)
        - .floating-content::after{content:''; clear: both; display: cblock}
        - BFC method: .floating-content{overflow: hidden/auto}
	- LVHA defining order: `:link`, `:visited`, `:hover`, `:active`
    - Flexbox
    - Grid
    - SVG
    - Transition & Animation:
        - [https://www.cnblogs.com/xiaohuochai/p/5347930.html](https://www.cnblogs.com/xiaohuochai/p/5347930.html)
        - [https://www.cnblogs.com/xiaohuochai/p/5391663.html](https://www.cnblogs.com/xiaohuochai/p/5391663.html)
---
- Techniques & Gotchas
    - Center
        - Horizontal
            - Inline/inline-block: 子父元素宽度固定，父元素设置 text-align: center; 子元素设置 display: inline-block; 子元素不能设置浮动，否则居中失效。如果将元素设置为 inline , 则元素的宽高设置会失效，这就需要内容来撑起盒子
            - block: 水平居中(margin: auto;)子父元素宽度固定，子元素上设置 margin: auto; 子元素不能设置浮动，否则居中失效。
        - Vertical
            - position: absolute; top: 50%; left: 50%; transform: translateX(50%) translateY(50%);
            - flex justfy-content(main line)/align-items(cross line): center;
    - Height equals dynamic width
        - width: 25%; height: 0; padding-bottom: 25%;
    - Position Gotchas:
        - static: top, bottom, left, right **won’t** work
        - z-index only works on positioned elements (absolute, relative, fixed, sticky, not static) and flex items (elements that are direct children of display:flex elements)
        - relative: z-index, top, bottom, left, right **will** work. children that are positioned elements will act based on relative elements.
---
- Resources & Tools
    [https://cssgr.id/](https://cssgr.id/) grid generator
    [https://www.cssfilters.co/](https://www.cssfilters.co/) CSS filter generator
    [https://flatuicolors.com/](https://flatuicolors.com/) palette design
    [http://zhongguose.com/](http://zhongguose.com/) Chinese colors
    [https://www.sj520.cn/tools/jianbian/](https://www.sj520.cn/tools/jianbian/) gradient design
    [https://daneden.github.io/animate.css/](https://daneden.github.io/animate.css/) animate.css