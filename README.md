# cssBox
    css 盒模型
    基本概念：标准模型+IE模型

    标准模型和IE模型区别
        标准模型只有content
        IE模型 content+padding+border   
    css如何设置这两者模型
        box-sizing:content-box
        box-sizing:border-box
    js如何设置获取盒模型对应的宽和高
        dom.style.width/height(只能去内联样式表的宽和高，也就是在元素上写的样式)
        dom.currentStyle.width/height(渲染之后的宽和高，无论在什么情况下，缺点只有ie支持)
        window.getComputedStyle(dom).width/height(渲染之后的宽和高，无论在什么情况下，缺点chrome和fif)
        dom.getBoundingClientRect().width/height(渲染之后的宽和高,ie5以上都能支持，但是又一点点地方需要修正一下，IE67的left、top会少2px,并且没有width、height属性。)
    根据盒模型解释边距重叠
        两种情况   
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
            <style>
                *{
                    padding: 0;
                    margin: 0;
                }
                section{
                    background-color: red;
                }
                article{
                    margin-top: 10px;
                    height: 100px;
                    background-color: yellow;
                }
            </style>
        </head>
        <body>
            <section>
                <article>
                    
                </article>
            </section>
        </body>
        </html>
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
            <style>
                *{
                    padding: 0;
                    margin: 0;
                }
                section{
                    overflow: hidden;
                    background-color: red;
                }
                article{
                    margin-top: 10px;
                    height: 100px;
                    background-color: yellow;
                }
            </style>
        </head>
        <body>
            <section>
                <article>
                    
                </article>
            </section>
        </body>
        </html>
    bfc(边距重叠解决方案)
         bfc的基本概念
            块级格式化上下文
         bfc的原理
            1）内部的Box会在垂直方向，一个接一个地放置。
            2）Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin不会发生重叠
            3）每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
            4）BFC的区域不会与float box重叠。
            5）BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
            6）计算BFC的高度时，浮动元素也参与计算
         如何创建bfc
            1）根元素
            2）float属性不为none
            3）position不为static和relative
            4）overflow不为visible
            5）display为inline-block, table-cell, table-caption, flex, inline-flex
         bfc的使用场景  
            1）防止外边距重叠。
            bfc导致的属于同一个bfc中的子元素的margin重叠(Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠)
            我们可以在div外面包裹一层容器，并触发该容器生成一个BFC。那么两个div便不属于同一个BFC，就不会发生margin重叠了。
            2）清除浮动的影响
            块级子元素浮动，如果块级父元素没有设置高度，其会有高度塌陷的情况发生。
            原因：子元素浮动后，均开启了BFC，父元素不会被子元素撑开。
            解决方法：由第六条原理得，计算BFC的高度时，浮动元素也参与计算。所以只要将父容器设置为bfc
            就可以把子元素包含进去：这个容器将包含浮动的子元素，它的高度将扩展到可以包含它的
            子元素，在这个BFC，这些元素将会回到页面的常规文档流。
            3）防止文字环绕
