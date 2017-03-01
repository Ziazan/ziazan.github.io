---
title: '[React学习]  业务中模块的拆分'
date: 2017-03-01 15:14:07
tags: [react]
categeories: [react]
---
# [React学习]  业务中模块的拆分

根据官网的例子 [React编程思想](http://reactjs.cn/react/docs/thinking-in-react-zh-CN.html)
*props* 是一种从父级传递数据到子级的方式。
### Example
父级:
```javascript
var FilterableProductTable  = React.createClass({
  render: function(){
  return (
    <div>
      <SearchBar />
      <ProductTable products={this.props.products} />
    </div>
  )
}
});
```
子级:
```javascript
var ProductTable = React.createClass({
    render: function(){
      var rows = [];
      var lastCategory = null;
      this.props.products.forEach(function(product){  //通过this.props.products 来获取父级传过来的数据
        if(product.category !== lastCategory){
          rows.push(
            // 插入分类标题的头部
            <ProductCategoryRow category={product.category} key={product.category} />
          )
        }
        rows.push(
          <ProductRow product={product} key={product.name} />
        );
        lastCategory = product.category;
      });
      return (
        <table className="productTable">
          <thead>
            <tr>
              <th>Name</th>
              <th>Price</th>
            </tr>
          </thead>
          <tbody>{rows}</tbody>
        </table>
      )
    }
});
```
### 根据官网示例写的代码（codepen）
<p data-height="265" data-theme-id="0" data-slug-hash="yMYwXm" data-default-tab="js" data-user="ziazan" data-embed-version="2" data-pen-title="React-demo1" class="codepen">See the Pen <a href="http://codepen.io/ziazan/pen/yMYwXm/">React-demo1</a> by ziazan (<a href="http://codepen.io/ziazan">@ziazan</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### 收获
在根据UI拆分模块的时候，根据数据模型进行拆分。
代码编写的时候，在构建项目的时候， 从顶层往下构建会比较容易一点。

-FilterableProductTable  (整体)
 -SearchBar（输入搜索框）
 -ProductTable（显示的数据表格）
   -ProductCategoryRow（分类名/列表头）
   -ProductRow（每一行的商品）



