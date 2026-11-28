## 项目地址

前端：https://github.com/LSTM-Kirigaya/mmd-fe

后端：https://github.com/guanhuhao/ShareMiracle.git

## 测试接口

测试机外网 ip 是 139.196.106.190

测试机的内网连接方式：

```bash
ssh gcsj@172.16.120.234 -p 3000
```


## 学习笔记

https://da1oso07lwd.feishu.cn/wiki/DxXxwyoVWihXyRkEHRPc4xHfnue

---

## TODO

写给自己看的 TODO

- [ x ] 将完成的后端 API 接入前端
- [ x ] 完善基础的数据结构
- [ x ] 完成基本的 30 多个数据集的 tag 和元信息

---

## 整体架构

架构采用 CSR 的方式进行组织（架构图进化中 ...）

```mermaid
graph LR
a(前端 vue3+ts) <--> b(后端 springboot) <--> c(数据库 mysql + mangodb)
```

---

### FE: mmd-fe

- 项目地址：https://github.com/LSTM-Kirigaya/mmd-fe
- 项目架构如下

```text
src
├─📁api         后端 API
├─📁assets      静态资源
│  ├─📁css      层叠样式表
│  ├─📁fonts    字体
│  └─📁image    图像
├─📁components  自定义全局通用组件
├─📁hook        自定义函数库
├─📁i18n        多语言配置
├─📁mock        mock数据配置
├─📁router      路由
├─📁store       状态库
├─📁types       全局自定义 type
└─📁views       路由组件
```

---

### BE: ShareMiracleApp

- 项目地址：https://github.com/guanhuhao/ShareMiracle.git
- 项目架构如下

```text
ShareMiracle-common 存放项目的通用工具
├─📁constant    常量
├─📁context     存放上下文信息（当前用户id）
├─📁exception   存放自定义异常类
├─📁json        对象映射器  基于jackson将Java对象转为json，或将json转为Java对象
├─📁properties  配置信息类（例如阿里云信息、jwt信息配置）
├─📁result      返回结果封装类 统一返回
└─📁utils       存放工具类 (如阿里云工具类、JWT工具类等)


ShareMiracle-pojo 存放项目中使用到的dto（传输）、vo（前端展示）、entity（实体）
├─📁 dto        前端信息传输的实体类
├─📁 vo         封装返回给前端展示的实体类
└─📁 entity     实体信息（与数据库对应）

ShareMiracle-server 存放项目具体的服务相关类
├─📁 config      存放配置信息类（如swagger测试文档配置、redis配置等；类命名规范：用途+Config（如MyBatisConfig）
├─📁 controller  存放控制器相关实现类；类命名规范：模块+Controller（如UserController）
├─📁 handler     存放全局处理器（如全局异常处理器，处理项目中抛出的业务异常）
├─📁 interceptor 存放全局拦截器（对所有请求进行拦截处理，根据具体的拦截处理逻辑，实现自定义拦截器）
├─📁 mapper      存放Mapper接口；类命名规范：模块+Mapper（如UserMapper）
├─📁 service     存放服务接口:命名规范：模块+Service（如UserService）
└─📁 impl        存放服务接口对应的实现类:命名规范：模块+ServiceImpl（如UserService）
```

---

## 接口设计

### 返回值规范

json格式

- code 状态码：如200、400等
- data 传递的数据：
- msg  传递的信息：可以为空

```json
{
  "code": 0,
  "data": ,
  "msg": ""
}
```

---

### 数据接口

用于描述一个数据集的

```json
{
    "id": 51,
    "name": "ASOCA",
    "origin_url": "https://xxx.xxx",
    "description": "xxxx",
    "release_date": "2024.05.21",
    "task_ids": [10],
    "modality_ids": [0, 1],
    "organ_ids": [0, 1],
    "data_num": 114514,
    "label_num": 0,
    "split_info": {
        "train_num": 114514,
        "val_num": 0,
        "test_num": 0
    }
}
```

Task id 

```json
{
    "0": "others",
    "1": "classification",
    "2": "segmentation",
    "3": "detection",
    "4": "reconstruction",
    "5": "superresolution",
    "6": "registration",
}
```

Modality id

```json
{
    "0": "others",
    "1": "common",
    "2": "ct",
    "3": "mri",
    "4": "xray",
    "5": "pet",
    "6": "ultrasonic",
    "7": "wsi",
    "8": "ehr",
    "9": "biomarker"
}
```

organ id 根据 total seg 的数据集进行划分

```json
{
    "1": "脾",
    "2": "右肾",
    "3": "左肾",
    "4": "胆囊",
    "5": "肝",
    "6": "胃",
    "7": "胰腺",
    "8": "右肾上腺",
    "9": "左肾上腺",
    "10": "左上肺叶",
    "11": "左下肺叶",
    "12": "右上肺叶",
    "13": "右中肺叶",
    "14": "右下肺叶",
    "15": "食管",
    "16": "气管",
    "17": "甲状腺",
    "18": "小肠",
    "19": "十二指肠",
    "20": "结肠",
    "21": "膀胱",
    "22": "前列腺",
    "23": "左肾囊肿",
    "24": "右肾囊肿",
    "25": "骶骨",
    "26": "S1椎",
    "27": "L5椎",
    "28": "L4椎",
    "29": "L3椎",
    "30": "L2椎",
    "31": "L1椎",
    "32": "T12椎",
    "33": "T11椎",
    "34": "T10椎",
    "35": "T9椎",
    "36": "T8椎",
    "37": "T7椎",
    "38": "T6椎",
    "39": "T5椎",
    "40": "T4椎",
    "41": "T3椎",
    "42": "T2椎",
    "43": "T1椎",
    "44": "C7椎",
    "45": "C6椎",
    "46": "C5椎",
    "47": "C4椎",
    "48": "C3椎",
    "49": "C2椎",
    "50": "C1椎",
    "51": "心脏",
    "52": "主动脉",
    "53": "肺静脉",
    "54": "头臂干",
    "55": "右锁骨下动脉",
    "56": "左锁骨下动脉",
    "57": "右颈总动脉",
    "58": "左颈总动脉",
    "59": "左头臂静脉",
    "60": "右头臂静脉",
    "61": "左心耳",
    "62": "上腔静脉",
    "63": "下腔静脉",
    "64": "门静脉",
    "65": "左髂动脉",
    "66": "右髂动脉",
    "67": "左髂静脉",
    "68": "右髂静脉",
    "69": "左肱骨",
    "70": "右肱骨",
    "71": "左肩胛骨",
    "72": "右肩胛骨",
    "73": "左锁骨",
    "74": "右锁骨",
    "75": "左股骨",
    "76": "右股骨",
    "77": "左髋骨",
    "78": "右髋骨",
    "79": "脊髓",
    "80": "左臀大肌",
    "81": "右臀大肌",
    "82": "左臀中肌",
    "83": "右臀中肌",
    "84": "左臀小肌",
    "85": "右臀小肌",
    "86": "左腰肌",
    "87": "右腰肌",
    "88": "左髂腰肌",
    "89": "右髂腰肌",
    "90": "大脑",
    "91": "颅骨",
    "92": "左肋骨",
    "93": "左肋骨",
    "94": "左肋骨",
    "95": "左肋骨",
    "96": "左肋骨",
    "97": "左肋骨",
    "98": "左肋骨",
    "99": "左肋骨",
    "100": "左肋骨",
    "101": "左肋骨",
    "102": "左肋骨",
    "103": "左肋骨",
    "104": "右肋骨",
    "105": "右肋骨",
    "106": "右肋骨",
    "107": "右肋骨",
    "108": "右肋骨",
    "109": "右肋骨",
    "110": "右肋骨",
    "111": "右肋骨",
    "112": "右肋骨",
    "113": "右肋骨",
    "114": "右肋骨",
    "115": "右肋骨",
    "116": "胸骨",
    "117": "肋软骨"
}
```

---

## /mdata/query-datasets-info

GET

```json
{
    "datasets": [
        {
            "id": 51,
            "name": "ASOCA",
            "origin_url": "https://xxx.xxx",
            "description": "xxxx",
            "release_date": "2024.05.21",
            "task_ids": [10],
            "modality_ids": [0, 1],
            "organ_ids": [0, 1],
            "data_num": 114514,
            "label_num": 0,
            "split_info": {
                "train_num": 114514,
                "val_num": 0,
                "test_num": 0
            }
        },
        {
            "id": 51,
            "name": "ASOCA",
            "origin_url": "https://xxx.xxx",
            "description": "xxxx",
            "release_date": "2024.05.21",
            "task_ids": [10],
            "modality_ids": [0, 1],
            "organ_ids": [0, 1],
            "data_num": 114514,
            "label_num": 0,
            "split_info": {
                "train_num": 114514,
                "val_num": 0,
                "test_num": 0
            }
        },
        ...
        {
            "id": 51,
            "name": "ASOCA",
            "origin_url": "https://xxx.xxx",
            "description": "xxxx",
            "release_date": "2024.05.21",
            "task_ids": [10],
            "modality_ids": [0, 1],
            "organ_ids": [0, 1],
            "data_num": 114514,
            "label_num": 0,
            "split_info": {
                "train_num": 114514,
                "val_num": 0,
                "test_num": 0
            }
        }
    ]
}
```

---

## /mdata/query-dataset-by-id

GET

```json
{
    "id": 51,
    "name": "ASOCA",
    "origin_url": "https://xxx.xxx",
    "description": "xxxx",
    "release_date": "2024.05.21",
    "task_ids": [10],
    "modality_ids": [0, 1],
    "organ_ids": [0, 1],
    "data_num": 114514,
    "label_num": 0,
    "split_info": {
        "train_num": 114514,
        "val_num": 0,
        "test_num": 0
    },
    "document": "xxxxxxx\nadfadawwwdwadwadaw\nadasadawdawdadwdawadawadwa"
}
```

---

## /mdata/get-small-samplev

GET

```json
{
    "samples": [
        {
            "modality": 0,
            "data_url": "http://xxx.xxx"
        },
        {
            "modality": 0,
            "data_url": "http://xxx.xxx"
        },
        {
            "modality": 0,
            "data_url": "http://xxx.xxx"
        }
    ]
}
```