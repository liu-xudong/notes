# CSS

### 美化滚动条样式

```css
.box {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    overflow: auto;
}

.box::-webkit-scrollbar {
    /*滚动条整体样式*/
    width: 5px;
    /*高宽分别对应横竖滚动条的尺寸*/
    height: 1px;
}

.box::-webkit-scrollbar-thumb {
    /*滚动条里面小方块*/
    border-radius: 5px;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    background: #535353;
}

.box::-webkit-scrollbar-track {
    /*滚动条里面轨道*/
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    border-radius: 5px;
    background: #ededed;
}
```

### 屏幕适配

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        h1 {
            width: 70%;
        }
        @media (max-width: 500px) {
            h1 {
                width: 100%;
            }
        }
    </style>
</head>
```

