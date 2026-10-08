# 青松坐姿

> ——基于设备端推理的校园久坐姿态识别与健康提醒系统

## 项目简介

本项目针对高校学生及办公人群长期久坐引发的颈椎及腰椎健康问题的现状，名为“青松坐姿”，是一款隐私保护型健康管理平台。

有别于传统穿戴式硬件或需上传云端的视觉产品，本项目基于 Web 设备端推理 AI 架构（WebAssembly+GPU加速），利用轻量级 MediaPipe 骨骼关键点提取算法，在本地浏览器闭环完成“低头、前倾、侧倾”等多角度姿态识别与防抖判定。

项目不仅融合了眨眼监测、坐姿不良全屏强制调整等柔性交互机制，以及无感化的“在座状态智能计时”模型。产品主打零硬件、免安装及严格的数据隐私保护，旨在探索轻量级计算机视觉在数字健康习惯养成领域的创新落地，最终成果将包含跨平台可运行原型、软件著作权及算法鲁棒性测试报告。



## TODO LIST


- [ ] 多角度监测：正面/侧面双模式

- [ ] （跨平台使用：Web/安卓/鸿蒙/PC）

- [ ] 历史记录：每日评分，日 / 周 / 月趋势，每周总结，帮你保持连续记录

- [ ] 学习模式：抬头提醒

- [ ] （体感游戏：参考bodysee）

- [ ] 国内市场+免费开源

- [ ] 前端：TS+VUE3

- [ ] 识别：MediaPipe 

- [ ] 隐私保护：本地运行，数据不上传

- [ ] 眨眼监测，眨眼提醒

- [ ] （多模态监测：红外/可穿戴 IMU陀螺仪）

- [ ] 自动开启护眼模式

- [ ] 驼背自动全屏休息，挺直恢复

- [ ] 计时逻辑：只算真正用电脑的时间，离开座位，计时就重新归零

- [ ] 人体工学意识工具，而非医疗护理产品

- [ ] 初次使用，自动校准

  

## 参考项目：
  
[NeckCure](https://neckcure.easyfox.org/zh-cn/#top)

[bodysee](https://github.com/soluckysummer/bodysee)

[czur](https://www.czur.com/cn/product/mirror)_

[pose-monitor](https://github.com/linyiLYi/pose-monitor)

[plumbcoach](https://plumbcoach.com/)

[posturekind](https://posturekind.com/)

[](https://apps.apple.com/cn/app/%E5%A5%BD%E5%9D%90%E5%A7%BF-%E7%BA%A0%E6%AD%A3%E5%9D%90%E5%A7%BF%E9%98%B2%E4%B9%85%E5%9D%90/id6745596088)