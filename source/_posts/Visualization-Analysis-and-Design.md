---
title: Visualization_Analysis_and_Design
categories: "可视化学习笔记"
tags: 
  - 可视化
  - gra_stu
language: zh-CN
---
# Visualization Analysis and Design

## Preface

- 这本书重点是在原则和设计选择方面广泛综合vis的一般基础，为**技术的设计和分析**提供一个框架，而非具体的方法。

  侧重于设计的抽象和习惯用法层次，而不设计域情况层次和算法层次

- 整书结构

> 第一章：vis定义，what-why-how
>
> 第二章：什么是数据抽象
>
> 第三章：为什么任务抽象
>
> 第四章：四个层次（最上层的领域情况层、最下层的算法层、数据和任务抽象的什么/为什么层、视觉编码和交互习惯用法设计）
>
> 第五章：编码信息、标记和通道的原理
>
> 第六章：设计的8条经验法则
>
> 接下来三章，如何安排空间来可视化编码数据
>
> 第七章：表格
>
> 第八章：空间数据
>
> 第九章：网络
>
> 第十章：视觉编码中映射颜色和其他通道的选择
>
> 第十一章：操作和更改视图的方法
>
> 第十二章：面向多维视图的方法
>
> 第十三章：减少数据项和数据属性
>
> 第十四章：嵌入技术
>
> 第十四章：概述数据上下文中嵌入焦点集的信息
>
> 第十五章：总结本书6个案例，详细分析了完整的框架

## 一、What's Vis,and Why Do it?

> <font color="#ff0000">**个人总结：**</font>vis是一种工具，用来增强人的能力而非完全代替人类，通过vis可以更好的分析和理解问题

### 1.1 The Big Picture

**为什么**用户需要它，显示了**什么数据**，数据（习语）**如何设计**的

### 1.2 Why Have a Human in the loop？

- If a fully automatic solution has been deemed to be acceptable,then there is no need for human judgement,and thus no need for you to design a vis tool.（如果一个全自动的解决方案被认为是可以接受的，那么就不需要人类的判断，因此你就不需要设计一个vis工具）

- However,many analysis problems are ill specified:people don't know how to approach the problem.（然而，许多分析问题是不明确的：人们不知道如何处理这个问题）

- augment human capabilites,rather than completely replace    the human in the loop.（增加人类的能力，而不是完全替代人类）

- 过渡性使用：前期对于分析需求获得更清晰的理解，中期为设计者改进、调试、扩展系统算法，后期为用户设计一个可视化工具

  > - In a highly exploratory way,gaining a clearer understanding of analysis requirements.
  > - In the middle stages of a transition,refine,debug,or  extend that system's algorithms for designers.
  > - you can also design a vis tool for end users.

- 长期使用

  > - speed up and imporve a user's ability to generate and check hypotheses.
  > - for presentation

### 1.3 Why have a Computer in the loop?

- big datasets
- tiny static datasets
- many datasets change dynamically over time.（许多数据集会随时间动态变化）

### 1.4 Why use an external representation?

- Diagrams can be designed to support preceptual inference.

### 1.5 Why depend on vison?

- The visual system provides a very high-handwidth channel to our brains.（视觉系统提供了一个非常高宽带的通道）
- Sound is poorly suited for providing overviews of large information spaces compared with vision.（与视觉相比，声音不适合提供大量信息空间的概述）
- The other sense can be immediately ruled out as communication channels because of technological limitations.（由于技术限制，其他感官可以立即派出作为沟通渠道的可能性）

### 1.6 Why show the data in detail?

- A single summary is often an oversimplification that hides the true structure of the dataset.（一个简单的总结往往是一个过度简化的过程，它隐藏了数据集的真实结构）

### 1.7 Why use interactivity?

- When datasets are large enough, the limitations of both people and displays preclude just showing everything at once.（当数据集很大时，人员和显示器的限制使得不能一次显示所有内容）
- interaction where user actions cause the view to change is the way forward.（用户操作使视图改变的交互是前进的方向）
- interactively changing display supports many possible queries.（交互式改变显示支持更多的可能性）

### 1.8 Why is the vis idiom design space huge？

- The design space of possibilities gets even bigger when  you consider how to manipulate one or more of these pictures with interaction.（当你考虑如何通过交互操作一张或多张图片时，可能性的设计空间就会变得更大）

### 1.9 Why focus on tasks?

以任务为驱动，每个任务对应方法和数据都不一样

### 1.10 Why focus on effectiveness?

- The focus on effectiveness is a corollary of defining vis to have the goal of supporting user tasks.（对效率的关注是定义vis以支持用户任务为目标的必然结果）

### 1.11 Why are most designs ineffective?

- Only a very small number of possibilities are in the set of reasonable choices, and of those only an even smaller fraction are excellent choices.（在合理选择的集合中，只有非常少的可能性，其中只有更小的一部分是优秀的选择）
- A more apporpriate goal when you design is to satisfy.（设计时更合适的目标是满足）

### 1.12 Why is validation difficult?

- There are so many questions that you could ask when considering whether a vis tool has met your design goals.(当考虑vis工具是否满足你的设计目标时，你你可以问很多问题)

### 1.13 Why are there resource limitations?

- you must consider at least three different kinds of limitations: computational capacity,human perceptual and cognitive capacity,and display capacity.（计算能力、人类感知和认知能力、显示能力）

  > - larger datasets
  >   - computer time and memory are limited resources.
  > - On the human side, memory and attention are finite  resources.(记忆和注意力)

### 1.14 Why analyze?

- analyzing existing systems is a good stepping stone to designing new ones.（分析现有系统是设计新系统的良好跳板）

- what? why? how?

  > - why the task being performed
  > - what data is shown in views
  > - how is the vis idiom constructed in terms of design choices

## 二、What:Data Abstraction

> <font color="#ff0000">**总结：**</font>介绍数据类型、数据集类型、属性类型；为什么需要数据语义和数据类型；主要解决是什么的问题

### 2.1 The Big picture

![image-20211026195021148](https://gitee.com/serendipity_LB/img/raw/master/image-20211026195021148.png)

> data、dataset、attributes(属性)
>
> - four basic dataset types:tables, networks, fields(字段), geometry(几何图形)
> - data types(5):items, attritutes, links, positions and grids.

### 2.2 Why do data semantics and types matter？(数据语义、数据类型)

- semantics of the data is its real-world meaning.
- type of the data is its structural or mathematical interpretation.

### 2.3 Data types

- **items**: an individual entity that is discrete(一个离散的个体实体)
- **attributes**：some specific property that can be measured,observed or logged.(测量、观察或记录的特定属性)
- **links**：a relationship between items, typically within a network(项目之间的关系，通常在网络内)
- **grid**：specifies the strategy for sampling continuous data in terms of both geometric and topological relationships between its cells.(指定根据单元之间的几何和拓扑关系对连续数据进行采样的策略)
- **position**：spatial data, providing a location in two-dimensional (2D) or three-dimensional (3D) space.(空间数据，提供二维或三维空间中的位置)

### 2.4 Dataset types

- **Tables**

> - be made up of rows and columns
> - item, cell, attribute

- **Networks**

> - an item in a network is often called a node
> - a link is a relation between two items
> - trees：with hierachical

- **Fields**

> - Spatial Fields(空间场)
> - Gird Types

- **Geometry**

> - the shape items with explicit spatial postions（明确空间位置的项的形状信息）
> - don't necessarily have attributes(不一定具有属性)

- **Other Combinations**

> - sets：an unordered group of items.(无序)
> - lists：a group of items with a specified ordering（有序）
> - clusters：a grouping based on attribute similarity（基于属性相似性的分组）

### 2.5 Attribute Types

> - The major disinction is between categorical versus ordered.（在于分类和顺序）
> - Attribute types are categorical, ordinal, or quantitative.(属性类型：分类、顺序、数量)
> - The direction of attribute ordering can be sequential, diverging, or cyclic.(属性排序方向：连续、离散、循环)

- **Categorical** 

> - can only distinguish whether two things are the same or different

- **Ordered**: Ordinal and Quantitative(序数和定量)

> - Sequential versus diverging(连续与发散)
> - Cyclic(循环)

- **Hierarchical Attributes(层次属性)**

### 2.6 Semantics

- **Key versus Value Semantics**（键值语义）

> - Flat Tables
> - Multidimensional Tables(多维表格)
> - Fields
>   - the number of keys versus values
> - Scalar Fields(标量场)
>   - a single value attribute at each point in space(在空间中每个点都有一个值属性)
> - Vector Fields(向量场)
> - Tensor Fields(张量场)
> - Field Semantic(字段语义)

- ​	**Temporal Semantics**（时序语义）
  - Time-Varying Data(时变数据)

## 三、Why:Task Abstraction

> <font color="#ff0000">**总结：**</font>以抽象的形式考虑任务，而不是通常认为的特定于领域的形式。主要分为Action和Target两部分。Action包括Analyze、Search和Query。其中Analyze分为：Comsume（仅利用现有的信息）和produce（除使用现有的信息外还生成新的信息），进一步细分又有：Discover、Present、Enjoy 和 Annotate、Record、Derive。其中Search基于对Target和Location的知道情况进一步分类。
>
> Target则是需要生成什么样的数据，达成什么样的任务，具体针对有All Data、Attributes、Network Data、Spatial Data

### 3.1 The Big Picture

![image-20211027085442641](https://gitee.com/serendipity_LB/img/raw/master/image-20211027085442641.png)

### 3.2 Why Analyze Tasks Abstractly?

- encourages you to consider tasks in abstract form,rather than the domain-specific way that users typically think about them.(鼓励你以抽象的形式考虑任务，而不是通常认为的特定于领域的方式)

### 3.3 Who:Designer or User

### 3.4 Actions

> independent from each other

#### 3.4.1 Analyze

- distinguish between two possible goals of people who want to analyze data using a vis tool：users might want only to **consume** existing information or also to actively **produce** new information.(用户可能只想使用已经存在的信息，或者也想产生新的信息)

> 1. **present** something that the user already understands to a third party(将用户理解的呈现给第三方)
> 2. **discover** something new or analyze information tha is not already completely  understood（发现一些新的东西或分析一些尚未完全理解的信息）
> 3. **enjoy** a vis to indulge their casual interests in a topic(在一个话题上尽情享受自己的兴趣)

#### 3.4.2 Produce

> 1. **Annotate(注释)**
> 2. **Record(记录)**：saves or captures visualization elements as persistent artifacts.(将可视化元素保存或捕获为持久构件)
> 3. **Derive(派生)**：produce new data elements based on existing data elements.(基于现有的数据元素生成新的数据元素)

#### 3.4.3 Search

> - All of the high-level analyze cases require the user to search for elements of interest within the vis as a **mid-level goal**

|                      | **Target known** | **Target unknown** |
| :------------------: | :--------------: | :----------------: |
|  **Location known**  |      Lookup      |       Browse       |
| **Location unknown** |      Locate      |      Explore       |

#### 3.4.4 Query

> Once a target or set of targets for a search has been found, a **low-level** user goal is to query these targets at one of three scopes: **identify**,**compare**, or **summarize**.

- **Identify**：a single target
- **Compare**：multiple targets
- **Summarize**：all possible targets

### 3.5 Targets

> - All Data：Trends，Outliers，Features
> - Attributes：One(Distribution、Extremes)，Many(Dependency、Correlation、Similarity)
> - Network Data：Topology(拓扑)、Paths
> - Spatial Data：Shape

### 3.6 How：A Preview

![image-20211027112826130](https://gitee.com/serendipity_LB/img/raw/master/image-20211027112826130.png)

Encode、Manipulate、Facet、Reduce

### 3.7 Analyzing and Deriving：Examples

> **what-why-how** framework can be used.

1. Comparing Two Idioms
2. Deriving One Attribute
3. Deriving Many New Attributes

## 四、Analysis：Four Levels for Validation

> <font color="#ff0000">**总结：**</font>可视化设计大概分为四个过程分别为：Domain situation、Data/Task abstraction、Visual encoding/Interaction idiom、Algorithm。由第一章所知验证十分麻烦，于是提出按层验证。具体可分为自顶向下or自底向上。同时提出各个层次会面对的Thread，以及如何validate

### 4.1 The Big Picture

![image-20211027141440920](https://gitee.com/serendipity_LB/img/raw/master/image-20211027141440920.png)

- **Task and Data abstraction**: addresses the **why and what** questions.
- **Idiom level**: addresses the question of **how**

### 4.2 Why Validate?

> - the vis design space is huge,and most designs are ineffective.(vis设计空间巨大，大多数设计无效)

### 4.3 Four Levels of Design

> - **Domain situation**：consider the details of a particular application domain for vis(考虑vis的特定应用领域的细节)
> - **Abstraction**：'what-why', map those domain-specific problems and data into forms that are independent of the domain.（特定于领域的问题和数据映射到独立于领域的表单中）
> - the design of idioms that specify the approach to visual **encoding and interaction**
> - design of algorithm

#### 4.3.1 Domain Situation

- a group of target users
- their domain of interest
- their questions
- their data

#### 4.3.2 Task and Data Abstraction

> abstracting the specific domain questions and data from the domain-specific form that they haveat the top level into **a generic representation**(抽象为通用的表达)

#### 4.3.3 Visual Encoding and Interaction Idiom

- **create and manipulate the visual representation of the abstract data block** that you chose at the previous level, guided by the abstract tasks that you also identified at that level（创建并操作在上一级选择的抽象数据块）
- two major concerns at play with idiom design
  1. how to create **a single picture** of the data：the visual encoding idiom controls exactly what users see（视觉编码习语精准控制用户看到的内容）
  2. how to manipulate that **representation dynamically**：the interactionidiom controls how users change what they see(交互习语控制用户如何更改他们所看到的内容)

#### 4.3.4 Algorithm

### 4.4 Angles of Attack

- **top down ：problem-driven work**
- **bottom up：technique-driven work**

### 4.5 Threat to Validity

![image-20211028191317538](https://gitee.com/serendipity_LB/img/raw/master/image-20211028191317538.png)

### 4.6 Validation Approaches

![image-20211028191609468](https://gitee.com/serendipity_LB/img/raw/master/image-20211028191609468.png)

## 五、Marks and Channels

> <font color="#ff0000">**个人总结：**</font>主要介绍可视化设计中的标记和信道。为什么学习信道？为分析可视编码提供建模块，即：为了更好的分析和设计可视化编码。其中Channel分为Magnitude（定量） 和Identity（定性），每个种类进而细分，对于信道效率判断有：依据其准确性分析、对比分析、独立分析、以及弹出式和分类分析。最后，由韦伯定律知，人类感知系统通常通常基于相对判断而非绝对判断。

### 5.1 The Big picture

![image-20211028210536125](https://gitee.com/serendipity_LB/img/raw/master/image-20211028210536125.png)

- **Marks** are basic geometric elements that depict items or links（描述项目和链接的基本几何元素）
- **Channels** control their appearance（控制外观）

### 5.2 Why Marks and Channels？

Learning to reason about marks and channels gives you the building blocks for analyzing visual encodings.(学习对标记和通道进行推理为分析可视化编码提供建模块)

### 5.3 Defining Marks and Channels

- **Marks：**geometric primitive objects classified according to the number of spatial dimensions they require
- **Channel：**a way to control the appearance of marks

#### 5.3.1 Channel Types

- **identity channels（定性）：**information about what something is or where it is
- **magnitude channels（定量）：**how much of something there is

### 5.4 Using Marks and Channels

1. Two principles guide the use of visual channels in visual encoding
   - **expressiveness** ：visual encoding should express all of, and only, the information in the dataset attributes.（可视化编码应该表达数据集属性中所有的信息）
   - **effectiveness**：the importance of the attribute should match the salience of the channel（属性重要性和信道一致性相匹配）
2. Channels Rankings

### 5.5 Channel Effectiveness

1. Accuracy
   - the obvious way to quantify effectiveness
2. Discriminability
3. Separability
4. Popout(弹出式)
5. Grouping

### 5.6 Relative versus Absolute Judgement(相对判断和绝对判断)

> **Weber's Law（韦伯定律）：**The human perceptual system is fundamentally based on relative judgement，not absolute ones（人类知觉系统基于相对判断而非绝对判断）

## 六、Rules of Thumb

> <font color="#ff0000">**个人总结：**</font>本章介绍了8大常用规则（技巧），分别为：针对3D、2D展示的各自优缺点；眼前所见优于记忆；分辨率大于沉浸感；概述大于细节；响应式是必需的；黑白分明；功能第一、形式第二

### 6.1 The Big Picture

![image-20211029160922330](https://gitee.com/serendipity_LB/img/raw/master/image-20211029160922330.png)

### 6.2 Why and When to Follow Rules of Thumb?

> A attempt to synthesize the current state of knowledge into a more unified whole（将当前知识状态合成一个更统一整体的尝试）

### 6.3 No Unjustified 3D

> 没有不合理的3D

- The Power of the Plane
- The Disparity of Depth(深度差异)
- Occlusion Hides Information(闭塞)
- Perspective Distortion Dangers(透视畸变)
- Tilted Text isn't Legibile(倾斜文字不清晰)
- Benefits of 3D：Shape Perception(形状感知)

### 6.4 No Unjustified 2D

### 6.5 Eyes Beat Memory

### 6.6 Resolution over Immersion

> 分辨率大于沉浸感

### 6.7 Overview First，Zoom and Filter，Details on Demand

> 首先是概述：缩放和过滤。按需要详解

- give the user a broad awareness of entire information space（使用户对整个信息空间有一个广泛的认识）

### 6.8 Responsiveness is Required

> 响应式是必需的

- The latency of interaction, namely, how much time it takes for the system to respond to the user action, matters immensely for interaction design.（交互延迟，即系统响应用户动作所花费的时间，对交互设计至关重要）

### 6.9 Get it Right inBlack and White

> 黑白分明

- That is, ensure that the most crucial aspects of visual representation are legible even if the image is transformed from full color to black and white.（确保视觉表现的最关键方面是易读的，即使图像从全彩转换为黑白）
- 确保足够的亮度对比度

### 6.10 Function First，Form Next

> 功能第一，表现次要

## 七、Arrange Tables

> <font color="#ff0000">**个人总结：**</font>如果安排平面数据（key-value）。主要分为四个部分：分别为表达值，可以直接量化；分类数据，可分为独立的、有序的、排列的；空间轴数据，可分为直线轴（即直角坐标系）、平行轴、圆形轴（极坐标系）；空间分布密度，可分为散点分布和空间填充。

### 7.1 The Big picture

![image-20211029203515594](https://gitee.com/serendipity_LB/img/raw/master/image-20211029203515594.png)

### 7.2 Why Arrange

- The arrange design choice covers all aspects of the use of spatial channels for visual encoding.（涵盖了使用空间通道进行视觉编码的所有方面）

### 7.3 Arrange by Keys and Values

- can show two value attributes：**scatterplots（散点图）**
- can show one key and one value：**bar charts（柱状图）**
- two keys and one values：**heatmaps（热图）**
- many keys and many values：**scatterplot matrices（散点图矩阵）**

### 7.4 Express: Quantitative Values

> 量化值

- A straightforward use of the spatial position channel to visually encode data.(空间位置信道来可视化编码数据的一种直接使用)

### 7.5 Separate，Order and Align：Categorical Regions

- breaking down the distribution of regions into three operations：s**eparating into regions，aligning the regions，and ordering the regions**.

1. List Alignment：One Key（列表）
2. Matrix Alignment：Two Keys（矩阵）
3. Volumetric Gird：Three Keys（三维网格）
4. Recursive Subdivision：Multiple Keys（递归细分）

### 7.6 Spatial Axis Orientation

1. Rectilinear Layouts（直线布局，即坐标轴）

2. Parallel Layouts（平行布局）

   Example：

   - Parallel Coordinates（平行坐标）

3. Radial Layouts（径向布局）

   除一个或多个线性空间信道外，还是用角度信道围绕一个圆分布。径向布局采用：极坐标系。

   Example：

   - Radial Bar Charts
   - Pie Charts

### 7.7 Spatial Layout Density

1. **Dense：**uses small and densely packed marks to provide an overview of as many items as possible with very high information density.（使用小而密集的标记来提供尽可能多的项目概述，信息密度高）
2. **Space-Filling：**has the property that it fills all available space in the view（空间填充）



## 八、Arrange Spatial Data

> <font color="#ff0000">**个人总结：**</font>主要介绍如何规划(安排)空间数据。首先空间数据可分为Geometry Data和Spatial Fields。其中Geometry主要为Geographic（地理数据信息）和其他数据派生而来的信息。空间字段又可分为标量场、向量和张量场。

### 8.1 The Big Picture

![image-20211030170803536](https://gitee.com/serendipity_LB/img/raw/master/image-20211030170803536.png)

>  空间数据类型：
>
>  1. 几何类型
>
>    - 地理数据
>    - 从其他数据派生而来的数据
>
>  2. 空间字段
>
>    - 每个场单元只有一个属性的**标量场**
>      - 等值线
>      - 直接体绘制
>    - **向量场和张量场**
>      - 局部信息的流符号
>      - 稀疏种子点集计算出几何形状的几何方法
>      - 密集种子集的纹理方法
>      - 整个空间域全局计算得出数据的特征方法

### 8.4 Scalar Fields：One Value

> - a single value associated with each each spatially defined cell.（与每个空间定义的单元格相关联的单个值）

### 8.5 Vector Fields：Multiple Values

> - are often associated with the application domain of computational fluid dynamics (CFD), as the outcome of flow simulations or measurements.（通常与计算流体动力学的应用领域联系在一起，作为流动模拟或测量的结果）

1. Flow Gyphs（符号流）
2. Geometric Flow（几何流）
3. Texture Flow（纹理流）
4. Feature Flow（特征流）

## 九、Arrange Networks and Trees

> <font color="#ff0000">**个人总结：**</font>介绍如何规划安排网络和树形数据。主要有三种形式，分别为：节点链接、邻接矩阵、包含（其中闭包不能用于图数据、只能用于树形，因为存在层次结构信息）。本章还针对邻接矩阵和及节点链接的优缺点展开了讨论。

### 9.1 The Big Picture

![image-20211031161150700](https://gitee.com/serendipity_LB/img/raw/master/image-20211031161150700.png)

### 9.4 Cost and Benefits：Connection versus Matrix

矩阵布局和节点链接布局的优缺点：

- 节点链接优点：小的网络可非常直观支持许多与网络数据相关的抽象任务
- 节点链接缺点：不适用于大型网络
- 矩阵试图优点：对大型和密集网络的感知可伸缩性；以及它们的可预测性、稳定性和对重新排序的支持
- 矩阵试图缺点：用户使用不熟悉；缺乏对拓扑结果研究的支持

### 9.5 Containment：Hierarchy Marks

> 包含：层次标记
>
> Containment vs Connection
>
> - containment：可显示层次结果的完整信息
> - connection：只显示两个item之间的成对关系

典型例子：树形图

## 十、Map Color and Other Channels

> <font color="#ff0000">**个人总结：**</font>本章主要从颜色和其他方面讨论如何映射。颜色方面分为颜色编码和颜色分类两大类，颜色编码引入HSL（即色调、饱和度、对比度），颜色映射又针对分类属性、有序属性、二元属性展开讨论。其他方面包括：大小、角度、曲率、形状、运动轨迹等等

### 10.1 The Big Picture

![image-20211031170317627](https://gitee.com/serendipity_LB/img/raw/master/image-20211031170317627.png)

> 1. 颜色
>    - 颜色编码
>      - Hue（色调）
>      - Saturation（饱和度）
>      - Luminance（亮度）
>    - 颜色映射
>      - 分类属性
>      - 有序属性
>        - 顺序
>        - 发散
>        - 二元
> 2. 大小、角度、曲度等
>    - 长度
>    - 角度
>    - 面积
>    - 曲率
>    - 体积
> 3. 形状
> 4. 运动轨迹

### 10.2 Color Theory

1. Color Vision

2. Color Spaces

   对RGB模型和HSL进行对比

3. Luminance，Saturation，and Hue

4. Transparency（透明度）

### 10.3 Colormaps

1. Categorical Colormaps

   - categories
   - grouping

2. Ordered Colormaps

   - ordinal attributes（序数属性）
   - quantitative attributes（数量属性）

3. Bivariate Colormaps


## 十一、Manipulate View

> <font color="#ff0000">**个人总结：**</font>本章介绍如果操作视图，主要分为随着时间改变视图，即动态改变；选择元素，如选择设计模式、提高亮度等；导航，又可分为对item reducetion 和 attribute reduction。对于项目有放缩、平移、约束；对于属性有切片、裁剪、投影。

### 11.1 The Big Picture

![image-20211101195234186](https://gitee.com/serendipity_LB/img/raw/master/image-20211101195234186.png)

### 11.2 Why Change？

为什么要将视图改变为随时间变化

- The most fundamental breakthrough of vis on a computer display compared with printed
  on paper is the possibility of interactivity: a view that changes over time can dynamically respond to user input, rather than being limited to a static visual encoding.(计算机显示的可视系统最根本突破就是可交互性。随时间变化的视图可以动态响应用户的输入，而不是局限于静态的视觉编码)
- all interactive idioms involve a view that changes over time.

### 11.3 Change View Over Time

- The visual encoding could be changed to a completely different idiom.(可视编码可以被改变成完全不同的习惯用法)
- alter some parameter of the existing encoding.(更改现有编码的某些参数)

### 11.4 Select Elements

1. Selection Design Choices
   - the number of independent selection type is also a design choices(独立选择的数量类型)
   - how many elements can be in the selection set.
2. Highlighting
3. Selection Outcomes

### 11.5 Navigate：Changing Viewpoint

1. Geometric Zooming(几何放缩)
2. Semantic Zooming(语义放缩)
3. Constrained Navigation(约束导航)

### 11.6 Navigate：Reducing Attributes

1. Slice

   例如最经典的：从三维转化为二维

2. Cut（裁剪）

3. Project(投影)

## 十二、Facet into Multiple View

> <font color="#ff0000">**个人总结：**</font>本章介绍面对多维视图如何设计。首先如何并置和协调多维视图，有：共享编码、共享数据（其中针对编码和数据二者关系，衍生出6种情形，其中2种情形冗余或无效）、共享导航（同步）；接下来介绍如何分区，以及对于不同的决策对比；最后介绍重叠层，从视觉分辨层、静态层和动态层出发，分别介绍各自优点。

### 12.1 The Big Picture

![image-20211102195240085](https://gitee.com/serendipity_LB/img/raw/master/image-20211102195240085.png)

### 12.3 Juxtapose and Coordinate Views

> 并置和协调视图

1. Share Encoding：Same/Different
2. Share Data：All，Subset，None
3. Share Navigation：Synchronize（同步）
4. Combinations
5. Juxtapose Views(并置视图)

### 12.4 Partition into Views

> 分区

1. Regions，Glyphs，and Views(区域符号和视图)

2. List Alignments（列表比对）

   > 关于不同的划分决策如何实现不同的任务，一种简单方法为：将分组条形图和多个对齐的条形图进行比较

3. Matrix Alignment（矩阵比对）

4. Recursive Subdivision(递归细分)

### 12.5 Superimpose Layers

> 重叠层

1. Visually Distinguishable Layers

   > 视觉分辨层。
   >
   > 使层可区分的一个方法为：确保每一层使用编码中活动的不同且不重叠的视觉通道范围。

2. Static Layers

   > 所有的图层同时显示，用户可以根据视觉注意的选择方向选择焦点。
   >
   > Example：
   >
   > - Cartographic Layering（地图分层）
   > - Superimposed Line Charts（叠加线图表）
   > - Hierarchical Edge Bundles（分层边缘包）

3. Dynamic Layers

   > 具有不同于视图其余部分的显著性的层是交互式构造的，通常是响应用户选择。

## 十三、Reduce Items and Attributes

> <font color="#ff0000">**个人总结：**</font>本章主要介绍如何减少项目和属性。主要分为过滤和聚合两种方法。

### 13.1 The Big Picture

![image-20211103112159279](https://gitee.com/serendipity_LB/img/raw/master/image-20211103112159279.png)

### 13.2 Why Reduce?

> - 通常静态数据减少习惯用法为只缩减显示的内容。然而，在动态环境下更改参数或选择的结果可能是元素数量的增加，即双向性。
> - Reducing the amount of data shown in a view is an obvious way to reduce its visual complexity.(减少视图中显示的数据量是降低其视觉复杂性最明显的方法)

### 13.3 Filter

> 挑战：在于如何确定好阈值

1. Item Filtering
2. Attribute Filtering

### 13.4 Aggregate

> 聚合：一组元素用一个新的派生元素来代表整组元素

1. Item Aggregation

2. Spatial Aggregation

3. Attribute Aggregation：Dimensionality Reduction

   >- Why and When to Use DR？
   >
   > preserve the meaningful structure of a dataset while using fewer attributes to represent the items.
   >
   >- How to Show DR data？
   >
   > 通过降维技术，用户可以选择要创建的合成属性的数量。当新属性的目标为2时，通常用散点图表示。创建属性大于2时，则常使用散点图矩阵。

## 十四、Embed：Focus+Context

> <font color="#ff000">**个人总结：**</font>本章主要介绍嵌入技术的用法。首先为什么要使用那内嵌，因为可以减轻标准导航技术带来的定向障碍。嵌入主要有Elide、Superimpose（）、Distort三种具体方法。最后分析嵌入技术的代价和价值之间的权衡，并引出5种显示复杂信息的方法。

### 14.1 The Big Picture

![image-20211103163925584](https://gitee.com/serendipity_LB/img/raw/master/image-20211103163925584.png)

### 14.2 Why Embed？

- mitigate the potential for disorientation that comes with standard navigation techniques such as geometric zooming.(减轻标准导航技术带来的定向障碍)

### 14.3 Elide

> 以一种动态的形式，从视图中完全忽略某些项。

### 14.4 Superimpose

重叠

> In this case，the focus layer is limited to a local region，rather than being a gobal layer that streches across the entire view to cover everthing.(焦点层仅局限局部区域，而不是扩展到整个视图以覆盖所有内容的全局层)

### 14.5 Distort

扭曲/失真

> some choice：
>
> - only a single region of focus(只有一个焦点区域) or allow multiple foci？（允许多个焦点）
> - the shape of the focus（焦点的形状）
> - the extent of the focus（焦点的范围）
> - interaction metaphor（交互隐喻）

> example：
>
> - 3D Perspective（三维透视）
> - Fisheye Lens（鱼眼镜头）：使用一个局部范围和径向相撞的单一焦点，以及主视图顶部的可拖动透镜的交互隐喻
> - Hyperbolic Geometry（双曲几何）：使用一个单一的径向全局焦点和双曲平移的相互作用
> - Stretch and Squish Navigation（拉伸和挤压导航）：使用全局范围的多个矩形焦点来实现失真，以及放大某些区域导致其他区域收缩。
> - Nonlinear Magnification Fields（非线性放大领域）

### 14.6 Cost and Benefits：Distortion

显示负责信息的五种选择：

1. Embed
2. derive new data（派生新数据）
3. manipulate a single changing view（操作单个更改视图）
4. facet into multiple views（将视图转换为多个视图）
5. reduce the amount of data to show.（减少显示的数据量）

以上五种方法之间的成本和收益权衡仍没有被完全理解

cost：

- distance or length judgements are severely impaired, so distortion is a poor match with any tasks that require such comparisons.（距离和长度的判断能力受到严重损害，因此失真与任何需要进行此类的任务都不匹配）

## 十五、Analysis Case Studies

### 15.1 The Big Picture

![image-20211103175618993](https://gitee.com/serendipity_LB/img/raw/master/image-20211103175618993.png)

> 1. Graph-Theoretic Scagnostic system：通过派生的SPLOM对原始散点图中的点分布形成的几何形状进行分类，从而提供大型散点图矩阵的可扩展总结
> 2. VisDB system：将整个数据库视为一个非常大的数据表，根据特定查询的相关性，用密集的、填充空间的布局对其进行可视化编码，并使用颜色条目
> 3. Hierarchical Clustering Explorer：支持对多维表的系统探索，以及相关的层次聚类
> 4. PivotGraph：使用派生网络的简洁数据抽象来总结网络
> 5. InterRing：使用一种空间填充、径向布局，并围绕多焦点+上下文失真建立交互作用
> 6. Constellation：支持浏览复杂的多层次语言网络，其布局将查询相关性与空间位置和动态分层进行编码，以避免边缘交叉的感知影响

