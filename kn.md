<!--
 * @Author: zxy
 * @Date: 2020-05-20 11:41:49
 * @LastEditTime: 2020-05-25 16:07:12
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \JSc:\Users\Administrator\Desktop\kn.md
--> 
<!--
 *                        _oo0oo_
 *                       o8888888o
 *                       88" . "88
 *                       (| -_- |)
 *                       0\  =  /0
 *                     ___/`---'\___
 *                   .' \\|     |// '.
 *                  / \\|||  :  |||// \
 *                 / _||||| -:- |||||- \
 *                |   | \\\  - /// |   |
 *                | \_|  ''\---/''  |_/ |
 *                \  .-\__  '-'  ___/-. /
 *              ___'. .'  /--.--\  `. .'___
 *           ."" '<  `.___\_<|>_/___.' >' "".
 *          | | :  `- \`.;`\ _ /`;.`/ - ` : | |
 *          \  \ `_.   \_ __\ /__ _/   .-` /  /
 *      =====`-.____`.___ \_____/___.-`___.-'=====
 *                        `=---='
 * 
 * 
 *      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 * 
 *            佛祖保佑       永不宕机     永无BUG
 * 
 *        佛曰:  
 *                写字楼里写字间，写字间里程序员；  
 *                程序人员写程序，又拿程序换酒钱。  
 *                酒醒只在网上坐，酒醉还来网下眠；  
 *                酒醉酒醒日复日，网上网下年复年。  
 *                但愿老死电脑间，不愿鞠躬老板前；  
 *                奔驰宝马贵者趣，公交自行程序员。  
 *                别人笑我忒疯癫，我笑自己命太贱；  
 *                不见满街漂亮妹，哪个归得程序员？
 -->
#### CSS元素居中

  ##### 1、水平居中

  - 对于行内元素：父级元素设置 ``text-align:center``

  - 对于确定宽度的块级元素

    - ``width`` 和 ``margin`` 的实现： 父级元素设置 ``margin:0 auto``

    - ``inline-block`` 实现水平居中方法：
    ``dispaly: inline-block; text-align: center``
    
    - 绝对定位和 ``margin-left`` 实现：
    父元素 ``position: relative; `` 子元素  ``position: absolute; margin-left: - width / 2``

  - 对于宽度未知的块级元素

    - ``table`` 标签配合 ``margin`` 实现：使用 ``table`` 标签或者直接将块级元素设值为 `` display:table``，再通过给标签添加左右 ``margin: 0 auto``

  

    - 绝对定位 + ``transform`` 实现:
    父元素 ``position: relative;`` 子元素 ``position: absolute; left: 50%;transform: translateX(-50%)``

    - ``flex`` 布局实现：``display: flex; justify-content: center``

  ##### 2、垂直居中

  - 利用 ``line-height`` 实现： 这种方法适合纯文字类

  - 通过设置父容器 ``相对定位``，子级设置 ``绝对定位``，标签通过 ``margin``实现自适应居中

  - 弹性布局 ``flex``，父级设置 ``display: flex``，子级设置 ``margin: auto 0``实现

  - 父级设置 ``相对定位``，子级设置 ``绝对定位``，通过 ``transform: translate``实现

  - ``table`` 布局，父级通过转换成表格形式，子级设置 ``vertical-align`` 实现。（``vertical-align: middle`` 使用的前提条件是内联元素以及 ``display: table-cell`` 的元素）
#### CSS布局

##### flex布局
##### 性能优化
##### 页面从输入url到渲染
##### 闭包、作用域
##### 原型链
##### 跨域
##### 防抖、节流
##### 数组排序、去重
##### 重绘、回流

##### vue双向绑定原理
##### webpack打包
##### 路由懒加载
##### promise、async、await
