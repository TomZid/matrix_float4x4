# 模型视图矩阵
模型视图矩阵是一个4*4矩阵，它表示一个变换后的坐标系，可以用来表示位置和对象的方向。为图元<sub>this</sub>提供的顶点将作为一个单列矩阵（column-majer storage order）（也就是一个向量）的形式使用，并乘以一个模型视图矩阵来获得一个相对于视觉坐标系的新坐标。
下图<sub>1.1</sub>所示为一个包含单个顶点数据的矩阵乘以模型视图矩阵后得到新的视觉坐标。顶点数据实际上是4个元素，包含一个缩放因子W。默认情况下这个值被设置为1.0，而且很少会改动。
![](http://image.ibb.co/mOtiOk/1.png)

通常会将一个顶点乘以矩阵来做变换。
### 矩阵构造
OpenGL并不是将一个4*4矩阵表示为一个浮点数的二维数组，而是将他表示为由16个浮点值组成的单个数组。这种方式与许多数学库都不同，这些数学库都使用二维数组的方式。例如，OpenGL会采用下面两个例子中的第一个：
GLfloat matrix[16];
GLfolat matrix[4][4];
OpenGL也可以使用第二种选择，但是第一种更高效。这16个元素表示4*4矩阵，如图<sub>1.2</sub>.当这些数组元素一个接一个的遍历矩阵中的列时，我们称为列优先排序(column-majer storage order)。而在存储器中这种4*4的二维数组（上述代码第二种选项）形式将以行优先排布。这两种形式的矩阵互为转置矩阵。
![](http://image.ibb.co/hh1sG5/2.png)

真正的奥妙在于，这16个值表示了空间中的一个特定位置，以及相对于视觉坐标系（屏幕坐标系，表现为top-left:(0,0),down-right:(1,1)，一个固定不变的坐标系，是应用到场景中的第一种变换）3个轴上的方向。这4列中每一列都代表一个由4个元素组成的向量。第4列向量包含变换后的坐标系原点的X、Y、Z值。
前3列的前3个元素只是表示方向向量，他们表示空间中X、Y、Z轴上的方向（用向量表示方向），对于大多数情况，这三个向量为单位向量且正交。请注意，矩阵最后一行都为0，只有最后一个元素为1。
![](http://preview.ibb.co/hHVV3k/3.png)
 
