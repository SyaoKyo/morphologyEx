# morphologyEx
这次尝试使用OpenCV2.14中的
<a class="reference external" href="http://opencv.jp/opencv-2.2_org/cpp/imgproc_image_filtering.html?highlight=morphology#morphologyEx">morphologyEx</a>函数进行进一步的形态变化：
<ul>
<li>开运算 (Opening)</li>
<li>闭运算 (Closing)</li>
<li>形态梯度 (Morphological Gradient)</li>
<li>顶帽 (Top Hat)</li>
<li>黑帽 (Black Hat)</li>
</ul>
<hr>
<h2>原理</h2>
<ol>
<li>开运算 (Opening)
<ul>
<li>开运算是通过先对图像腐蚀再膨胀实现的。
<div align=center>
<img src="./pic/db7b4c512c715fd8c98c9eb7ef280243550770db.png" alt="dst = open( src, element) = dilate( erode( src, element ) )"/>
</div>
</li>
<li>能够排除小团块物体(假设物体较背景明亮)</li>
<li>请看下面，左图是原图像，右图是采用开运算转换之后的结果图。 观察发现字母拐弯处的白色空间消失。
<div align=center>
<img alt="Opening" class="align-center" src="./pic/Morphology_2_Tutorial_Theory_Opening.png" />
</div>
</li>
</ul>
</li>
<li>闭运算 (Closing)
<ul>
<li>闭运算是通过先对图像膨胀再腐蚀实现的。
<div align=center>
<img src="./pic/79308d2c2365645f1ad5f2158a28f3b651b2bba4.png" alt="dst = close( src, element ) = erode( dilate( src, element ) )"/>
</div>
</li>
<li>能够排除小型黑洞(黑色区域)。
<div align=center>
<img alt="Closing example" class="align-center" src="./pic/Morphology_2_Tutorial_Theory_Closing.png" />
</div>
</li>
</ul>
</li>
<li>形态梯度 (Morphological Gradient)
<ul>
<li><p class="first">膨胀图与腐蚀图之差</p>
<div align=center>
<img src="./pic/64532c62837e670a0fe7088ea5766d34c984915e.png" alt="dst = morph_{grad}( src, element ) = dilate( src, element ) - erode( src, element )"/>
</div>
</li>
<li>能够保留物体的边缘轮廓，如下所示:
<div align=center>
<img alt="Gradient" class="align-center" src="./pic/Morphology_2_Tutorial_Theory_Gradient.png" />
</div>
</li>
</ul>
</li>
<li>顶帽 (Top Hat)
<ul>
<li>原图像与开运算结果图之差:
<div align=center>
<img src="./pic/7017f33204d4d9a80e7c08e74732fb7425b9a3f4.png" alt="dst = tophat( src, element ) = src - open( src, element )"/><br>
<img alt="Top Hat" src="./pic/Morphology_2_Tutorial_Theory_TopHat.png" />
</div>
</li>
</ul>
</li>
<li>黑帽 (Black Hat)
<ul>
<li>闭运算结果图与原图像之差:
<div align=center>
<img src="./pic/e45f9b0c3379529b821fa16a98464195272de1cd.png" alt="dst = blackhat( src, element ) = close( src, element ) - src"/><br>
<img alt="Black Hat" src="./pic/Morphology_2_Tutorial_Theory_BlackHat.png"/>
</div>
</li>
</ul>
</li>
</ol>
<hr>
<h2>代码解释</h2>
<code>
<pre>
void Morphology_Operations(int, void*)
{
	// 由于 MORPH_X的取值范围是: 2,3,4,5 和 6
	int operation = morph_operator + 2;
	Mat element = getStructuringElement(morph_elem, 
					Size(2 * morph_size + 1, 2 * morph_size + 1), 
					Point(morph_size, morph_size));
	// 运行指定形态学操作
	morphologyEx(src, dst, operation, element);
	imshow(window_name, dst);
}
</pre>
</code>
运行形态学操作的核心函数是 <a class="reference external" href="http://opencv.jp/opencv-2.2_org/cpp/imgproc_image_filtering.html?highlight=morphology#morphologyEx">morphologyEx</a> 。在本例中，我们使用了4个参数(其余使用默认值):</p>
<ul>
<li><strong>src</strong> : 原 (输入) 图像</li>
<li><strong>dst</strong>: 输出图像</li>
<li><strong>operation</strong>: 需要运行的形态学操作。 我们有5个选项:
<ul>
<li><em>Opening</em>: MORPH_OPEN : 2</li>
<li><em>Closing</em>: MORPH_CLOSE: 3</li>
<li><em>Gradient</em>: MORPH_GRADIENT: 4</li>
<li><em>Top Hat</em>: MORPH_TOPHAT: 5</li>
<li><em>Black Hat</em>: MORPH_BLACKHAT: 6</li>
</ul>	
<li><strong>element</strong>: 内核，可以使用函数:get_structuring_element:<cite>getStructuringElement &lt;&gt;</cite> 自定义。</li>
</ul>
