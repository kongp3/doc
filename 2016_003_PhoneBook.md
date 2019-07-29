# 1 引言

* 本文档通过对31.102规范附录G中的Phone Book结构示例图展开，对Phone Book的类型，结构及相关文件的内容进行详细说明。* 对于规范每个人都有自己的理解，如本文有理解错误的地方还请批评指出。

# 2	缩略语及参考文档
## 2.1 缩略语

<table>
<tr>
<td>EF</td><td>Elementary File 基本文件</td>
</tr>
<tr>
<td>FID</td><td>File Identifier 文件标识</td>
</tr>
<tr>
<td>SFI</td><td>Short File Identifier 短文件标识</td>
</tr>
<tr>
<td>PBR</td><td>Phone Book Reference 电话本参考</td>
</tr>
<tr>
<td>ADN</td><td>Abbreviated Dialing Number 缩位拨号号码</td>
</tr>
<tr>
<td>EXT</td><td>Extension 扩展</td>
</tr>
<tr>
<td>CCP</td><td>Capability Configuration Parameter 能力配置参数</td>
</tr>
<tr>
<td>GRP</td><td>Grouping file 分组文件</td>
</tr>
<tr>
<td>GAS</td><td>Grouping information Alpha String 组信息字符串</td>
</tr>
<tr>
<td>ANR</td><td>Additional Number 附加号码</td>
</tr>
<tr>
<td>ASS</td><td>Additional number Alpha String 附加号码字符串</td>
</tr>
<tr>
<td>SNE</td><td>Second Name Entry 第二名称条目</td>
</tr>
<tr>
<td>IAP</td><td>Index Administration Phone book 索引管理电话簿文件</td>
</tr>
<tr>
<td>PBC</td><td>Phone Book Control 电话本控制文件</td>
</tr>
<tr>
<td>UID</td><td>Unique Identifier 唯一标识</td>
</tr>
<tr>
<td>PUID</td><td>Previous Unique Identifier 前次UID</td>
</tr>
<tr>
<td>CC</td><td>Change Counter 变更计数器</td>
</tr>
<tr>
<td>PSC</td><td>Phone book Synchronisation Counter 电话簿同步计数器</td>
</tr>
</table>

## 2.2	参考文档
* [1] ETSI TS 131 102 V11.6.0 Universal Mobile Telecommunications System (UMTS)* [2] 中国联通USIM卡技术规范1204# 3	面向对象
## 3.1	广义的面向对象* 类：对有共同特征事物的统一描述* 对象：一类事物的特征实例化

## 3.2 PhoneBook的面向对象
* 类：规定每个电话本包含的数据对象、文件类型——不会体现在文件系统中，是建立文件系统前就规划好的(或者理解为是体现在PBR文件的第一条记录)* 对象：每个电话本数据格式，对象的大小，FID，SFI——体现在PBR文件内容和每个文件实体中
# 4 附录举例解析
## 4.1 附录图例(类)

{% img [class names] /path/to/origin [width] [height] [title text [alt text]] %}
1. 所有的箭头的出发点都是ADN2. ADN、PBC、ANR、EMAIL、SNE、UID、是规定的类别，本图所示项目中每套Phonebook都要有的。3. Email、SNE、UID、PBC、EXT1、GRP、ANR与ADN互相映射4. EXT1、GAS、AAS是公用的文件。5. GAS与GRP文件互相映射，AAS与ANR文件互相映射。

## 4.2 加标注的图例(类)

{% img [class names] /path/to/origin_with_comment [width] [height] [title text [alt text]] %}
1. 每个文件的类别都有对应的Tag来表示

## 4.3 抽象为PBR的图例(对象)
```
Rec1:A819C0034F3901 C5034F4109 C4034F5A03 C6034F5105 C9034F6107A90ACA034F710F C3034F7313AA0FC2034F4A0C C8034F4C0B C7034F4B0D
```

```Rec2:A819C0034F3A02 C5034F420A C4034F5B04 C6034F5206 C9034F6208 A90ACA034F7210 C3034F7414AA0FC2034F4A0C C8034F4C0B C7034F4B0D
```

```Rec3：…
```* 抽象的内容是如何得到的呢？

# 5 PhoneBook文件格式说明
* 这里标题为PhoneBook文件格式说明，其实实质是对5F3A/4F30文件格式的说明，把4F30拿到PhoneBook概念之外比较好理解，我们只认为4F30中定义的文件才是PhoneBook文件。

## 5.1	文件类型(PhoneBook总体结构)
<table>
<tr>
<td>标签值</td><td>结构化的标签描述</td>
</tr>
<tr>
<td>'A8'</td><td>类型1</br>1、此处定义的第一个文件标识被称为主基本文件</br>2、通常主基本文件都是ADN文件</br>3、所有类型1文件和ADN文件映射关系为：通过记录号一一映射</td>
</tr>
<tr>
<td>'A9'</td><td>类型2</br>1、这里的文件通过EFIAP文件管理的内容和EFADN文件的每条记录对应，详见章节6.10对IAP文件的介绍</br>2、所有类型2文件和ADN文件映射关系为：通过IAP文件内容映射</td>
</tr>
<tr>
<td>'AA'</td><td>类型3</br>1、类型3文件可以多组ADN文件共享</br>2、类型3文件与ADN的映射关系为：通过参考文件的指定来映射</td>
</tr>
</table>

## 5.2 文件Tag(PhoneBook具体文件)

<table>
<tr>
<td>标签值</td><td>标签定义</td>
</tr>
<tr>
<td>'C0'</td><td>EFADN 数据对象</td>
</tr>
<tr>
<td>'C1'</td><td>EFIAP 数据对象</td>
</tr>
<tr>
<td>'C2'</td><td>EFEXT1 数据对象</td>
</tr>
<tr>
<td>'C3'</td><td>EFSNE 数据对象</td>
</tr>
<tr>
<td>'C4'</td><td>EFANR 数据对象</td>
</tr>
<tr>
<td>'C5'</td><td>EFPBC 数据对象</td>
</tr>
<tr>
<td>'C6'</td><td>EFGRP 数据对象</td>
</tr>
<tr>
<td>'C7'</td><td>EFAAS 数据对象</td>
</tr>
<tr>
<td>'C8'</td><td>EFGAS 数据对象</td>
</tr>
<tr>
<td>'C9'</td><td>EFUID 数据对象</td>
</tr>
<tr>
<td>'CA'</td><td>EFEMAIL 数据对象</td>
</tr>
<tr>
<td>'CB'</td><td>EFCCP1 数据对象</td>
</tr>
</table>

## 5.3 文件类型和文件tag的匹配关系

* A8:C0,C1,C4,C5,C6,C9,CA* A9:C3,C4,CA* AA:C2,C7,C8,CB

## 5.4 内容

* 文件Tag后一定是有LV结构跟着，文件类型后的L包含此类型定义的内容总长;每个文件tag后的LV可以是03+FID+SFI，也可以是02+FID* 再看一眼之前抽象出来的数据```Rec1:A819C0034F3901 C5034F4109 C4034F5A03 C6034F5105 C9034F6107A90ACA034F710F C3034F7313AA0FC2034F4A0C C8034F4C0B C7034F4B0D``````
Rec2:A819C0034F3A02 C5034F420A C4034F5B04 C6034F5206 C9034F6208 A90ACA034F7210 C3034F7414AA0FC2034F4A0C C8034F4C0B C7034F4B0D```

