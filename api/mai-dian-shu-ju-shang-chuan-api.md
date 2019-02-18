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
      <th style="text-align:left">字段名称</th>
      <th style="text-align:left">是否必填</th>
      <th style="text-align:left">字段格式</th>
      <th style="text-align:left">字段说明</th>
      <th style="text-align:left">字段值示例</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">cs1</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">字符串</td>
      <td style="text-align:left">用户登录ID</td>
      <td style="text-align:left">9128391</td>
      <td style="text-align:left">
        <p>长度限制255,且字符串中不能包含":"</p>
        <p>invalidCS1: ["0", "1", "-1", "{<a href="http://user.id/">user.id</a>}",null]</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">tm</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">数值</td>
      <td style="text-align:left">事件发生时间时间戳（毫秒）</td>
      <td style="text-align:left">1506069592985</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">n</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">字符串</td>
      <td style="text-align:left">事件标识</td>
      <td style="text-align:left">intcmpEntry</td>
      <td style="text-align:left">长度限制255</td>
    </tr>
    <tr>
      <td style="text-align:left">var</td>
      <td style="text-align:left">否</td>
      <td style="text-align:left">对象</td>
      <td style="text-align:left">所有的自定义事件变量</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p>key：value</p>
        <p>value：若为字符串，长度限制255</p>
        <p>value：若为数值，可以为整数，小数（支持多个小数位）</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">t</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">字符串</td>
      <td style="text-align:left">固定cstm</td>
      <td style="text-align:left">cstm</td>
      <td style="text-align:left">固定</td>
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

