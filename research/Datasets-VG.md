# Visual Grounding Datasets

| Dataset   | Image/Query/Object Num | Split                                                        | Attribute |
| --------- | ---------------------- | ------------------------------------------------------------ | --------- |
| RefCOCO   | 19994/142210/50000     | **unc:** Train:120624, Val:10834, TestA:5657, TestB:5095 ~~**google: **Train:113762, Val:14246, Test: 14202~~ |           |
| RefCOCOg  | 25799/95010/49822      | **umd: **Train: 80512, Val: 4896, Test:9602 **google:** Train: 85474, Val: 9536, Test: - |           |
| RefCOCO+  | 19992/141564/49856     | **unc:** Train:120191, Val:10758, TestA:5726, TestB:4889     |           |
| ÍReferIt  | 19894/130525/96654     | cleaned:Train:54127, Val:5842, Test:60103                    |           |
| Flickr30k | 31783/427000           |                                                              |           |



RefCOCOg是google同时开展split工作与unc撞车的版本，unc为RefCOCO

后来在umd的文章Modeling Context Between Objects for Referring Expression Understanding中对google版本的RefCOCOg做了更新，因此RefCOCOg分为RefCOCOg-google和RefCOCOg-umd版本。

**unc: Modeling Context in Referring Expressions** 该文章定义了RefCOCO和RefCOCO+的TestA/TestB双测试集Splits



**确定使用：**

Modeling Context in Referring Expressions. ECCV 2016

- RefCOCO：unc split
- RefCOCO+：unc split
- RefCOCOg：umd
- ReferIt
- Flickr30k

