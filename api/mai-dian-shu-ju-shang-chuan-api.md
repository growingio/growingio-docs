# 埋点数据上传API（Beta）

#### 该功能还处于Beta测试阶段，如需使用，请联系您的客户成功经理

## **接口方法**

POST（GET方法后续支持）

## **数据接口URL**

https://api.growingio.com/v3/${ai}/s2s/cstm?stm=${sendingTime}

## **请求变量说明**

| 字段名称 | 字段说明 | 字段示例 |
| :--- | :--- | :--- |
|  ai |  项目ID |  参见GrowingIO产品中 _项目管理_ 模块 |
| ​ sendingTime | ​ 发送时间时间戳（毫秒） |  ​1506069592985 |
|  data |  本次事件的内容 |   json格式字符串，参考后续内容 |

## **Body格式：**

**单条事件发送** 

```text
[      
  {            
    "cs1":"9128391",    
    "tm":1434556935000,    
    "t":"cstm",    
    "n":"BuyProduct",    
    "var":{      
      "product_name":"苹果",      
      "product_classify":"水果",      
      "product_price":14    
    }
  }
]
```

**多条事件发送**

```text
[      
  {            
    "cs1":"9128391",    
    "tm":1434556935000,    
    "t":"cstm",    
    "n":"BuyProduct",    
    "var":{      
      "product_name":"苹果",      
      "product_classify":"水果",      
      "product_price":14    
    }
  },   
  {            
    "cs1":"9128391",    
    "tm":1434556935000,    
    "t":"cstm",    
    "n":"BuyProduct",    
    "var":{      
      "product_name":"苹果",      
      "product_classify":"水果",      
      "product_price":14    
    }
  },   
  {            
    "cs1":"9128391",    
    "tm":1434556935000,    
    "t":"cstm",    
    "n":"BuyProduct",    
    "var":{      
      "product_name":"苹果",      
      "product_classify":"水果",      
      "product_price":14    
    }
  }
]
```

## **Body字段说明**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5B57;&#x6BB5;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x5B57;&#x6BB5;&#x683C;&#x5F0F;</th>
      <th style="text-align:left">&#x5B57;&#x6BB5;&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x5B57;&#x6BB5;&#x503C;&#x793A;&#x4F8B;</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">cs1</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7528;&#x6237;&#x767B;&#x5F55;ID</td>
      <td style="text-align:left">9128391</td>
      <td style="text-align:left">
        <p>&#x957F;&#x5EA6;&#x9650;&#x5236;255,&#x4E14;&#x5B57;&#x7B26;&#x4E32;&#x4E2D;&#x4E0D;&#x80FD;&#x5305;&#x542B;&quot;:&quot;</p>
        <p>invalidCS1: [&quot;0&quot;, &quot;1&quot;, &quot;-1&quot;, &quot;{<a href="http://user.id/">user.id</a>}&quot;,null]</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">tm</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x65F6;&#x95F4;&#x65F6;&#x95F4;&#x6233;&#xFF08;&#x6BEB;&#x79D2;&#xFF09;</td>
      <td
      style="text-align:left">1506069592985</td>
        <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">n</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x4E8B;&#x4EF6;&#x6807;&#x8BC6;</td>
      <td style="text-align:left">intcmpEntry</td>
      <td style="text-align:left">&#x957F;&#x5EA6;&#x9650;&#x5236;255</td>
    </tr>
    <tr>
      <td style="text-align:left">var</td>
      <td style="text-align:left">&#x5426;</td>
      <td style="text-align:left">&#x5BF9;&#x8C61;</td>
      <td style="text-align:left">&#x6240;&#x6709;&#x7684;&#x81EA;&#x5B9A;&#x4E49;&#x4E8B;&#x4EF6;&#x53D8;&#x91CF;</td>
      <td
      style="text-align:left"></td>
        <td style="text-align:left">
          <p>key&#xFF1A;value</p>
          <p>value&#xFF1A;&#x82E5;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#xFF0C;&#x957F;&#x5EA6;&#x9650;&#x5236;255</p>
          <p>value&#xFF1A;&#x82E5;&#x4E3A;&#x6570;&#x503C;&#xFF0C;&#x53EF;&#x4EE5;&#x4E3A;&#x6574;&#x6570;&#xFF0C;&#x5C0F;&#x6570;&#xFF08;&#x652F;&#x6301;&#x591A;&#x4E2A;&#x5C0F;&#x6570;&#x4F4D;&#xFF09;</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">t</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x56FA;&#x5B9A;cstm</td>
      <td style="text-align:left">cstm</td>
      <td style="text-align:left">&#x56FA;&#x5B9A;</td>
    </tr>
  </tbody>
</table>

## 程序调试

GrowingIO建议您按照如下步骤进行埋点数据的开发联调

1. 在GrowingIO的网站中创建自定义事件以及对应的自定义事件变量
2. 参考上面的文档，编写一段程序上传一个事件，确保收到了200的HTTP状态
3. 在GrowingIO产品中使用您创建的埋点数据创建一个实时的图表
4. 如果可以在实时产品中看到您的埋点数据统计，则代表您的埋点创建成功

## 常见问题

为什么在GrowingIO中无法看到我上报的数据？

答：因为服务器端埋点事件无法关联访问用户，所以需要将目标用户修改为“全部登录用户“才能看到数据

