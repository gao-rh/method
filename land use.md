### 1. Adversarial transfer learning for migratable classification 

---

#### new words

leverage, pervasive, elaborate, judicious,

**adversarial**, **urban fabric**,  **scalable**, 

---

#### 概述

部分城市（记为城市B）的已标注的数据难以获得，而有些城市(记为城市A)数据有统计和公开，该文章提出了新方法，通过城市A的数据训练模型，用该模型，对非监督学习转化的B城市图像进行分类，对比不转换直接用常规模型训练，直接对另一个城市遥感图像进行分类的结果有显著提升，城市、森林部分分类结果还行（0.7-0.8），绿地，交通不行。

#### 模型和算法

模型分了三个部分，

- 第一部分为GMNR，将相似图片分组（同时包含源城市图片和目标城市图片），防止不同种类的图片特征迁移到了目标图片，例如将房屋特征（颜色等）迁移到了绿地。Grouping Mechanism for Noise Reduction 

- 第二部分CSIT，该部分模型loss有点意思，该部分为两个子部分，Cross-city Satellite Image Translation 
  - 一个为判别图片是否来自city A，***figure 3***
  - 一个为将city A的城市特征迁移到city B图片上，***figure 4***

- 第三部分为图片分类，land Usage Classification Module (LUCM)

  ![image-20220728195220856](land%20use.assets/image-20220728195220856.png)

  ![image-20220728195244000](land%20use.assets/image-20220728195244000.png)

算法

```pseudocode
input:A城市遥感图像(Sa)，B城市遥感图像(Sb),A城市分类结果(Ca),
output:B城市分类结果
流程：
通过GMNR将图片分组
通过CSIT对各组图片
	将B城市图像转换为A城市风格的图像Sb_translated
通过A城市的标注数据（Sa、Ca）训练得到分类模型M
用分类模型M对SB_translated进行分类得到B城市分类结果

```

----



#### 文章具体内容粘贴

goal

​	goal is to accurately classify the land usage of locations in a target city where the ground truth land usage data is not available by leveraging a classification model from a source city where such data is available.

​	城市中可获得的数据中，没有土地使用分类的真实数据，会导致大量工作在标注上。

problem

Two important challenges exist in solving our problem: 

- the target and source cities often have different urban  characteristics that prevent the direct application of a model learned from the source city to the target city; 

  不同城市有着不同的特点，导致从原城市迁移到目标城市有困难。

- the complex visual features in satellite images make it non-trivial to “translate” the images from the target city to the source city for an accurate classification.

  遥感图像的复杂的可视特征，使得从图片中分类困难

TransLand

​	提出了个新模型，可以解决上面两个问题。

​	TransLand is an adversarial transfer learning framework to translate the satellite images from the target city to the source city for accurate land usage classification. 

​	We evaluate our scheme on the real-world satellite imagery and land usage datasets collected from five different cities in Europe.

 The results  how that TransLand significantly outperforms the state-of-the-art land usage classification baselines in classifying the land usage of locations in a city.

##### 引入

- land usage classification is important.
- a rich set of ground-truth data on land usage is needed as training data to extract the features and build model.
- ground-truth data is hard to get. there is no such data in most city, so migratable land usage classification is needed
- learn the land usage classes of locations in Dortmund  where ground-truth data published by leveraging the land usage classification model learned in Berlin where no ground-truth data published.
- problem 
  - classification performance drop when the classification model learned from one city is directly applied to another.
  - accommodate the disparity by re-training the
    classification model.

下图为一种转换方式。

![image-20220727194350717](land%20use.assets/image-20220727194350717.png)

##### 模型



![](land%20use.assets/image-20220728195214103.png)

module

模型分了三个部分，

- 第一部分为非监督简单的分类器，将相似图片分组（同时包含源城市图片和目标城市图片）。Grouping Mechanism for Noise Reduction (GMNR)

- 第二部分为两个部分，Cross-city Satellite Image Translation (CSIT)
  - 一个为判别图片是否来自city A，***figure 3***
  - 一个为将city A的城市特征迁移到city B图片上，***figure 4***

- 第三部分为图片分类，land Usage Classification Module (LUCM)

第二部分loss有意思。





![image-20220728195220856](land%20use.assets/image-20220728195220856.png)

![image-20220728195244000](land%20use.assets/image-20220728195244000.png)

##### 结果

- 有提升
- 城市森林分类可以。



![image-20220729150348364](land%20use.assets/image-20220729150348364.png)





![image-20220728203109005](land%20use.assets/image-20220728203109005.png)

### 
