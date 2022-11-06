### 子盒子在父盒子中水平垂直居中

1. 定位 position+margin 外边距（需要知道子盒子的宽高）

   ```css
   /*父盒子*/
   position:relative;
   /*子盒子*/
   position:absolute;
   top:50%;
   left:50%;
   margin-top:-50px;
   margin-left:-50px;
   ```

2. 定位 position+transform（不需要知道宽高）

   ```css
   /*父盒子*/
   position:relative
   ```

   

