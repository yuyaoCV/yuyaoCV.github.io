---
layout: archive
title: "活动"
permalink: /blogs/
author_profile: true
redirect_from:
  - /wordpress/blog-posts/
---
<!-- {% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}
{% include base_path %}
{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %} -->



* **[2021/10]** 参加 *IEEE Big Data Science and Engineering 2021* 国际会议并作学术报告 <a href="/cv/images/bigdatase_yy.jpg">[IMG]</a>
* **[2021/09]** 担任 中国移动-第二届信息通信网络职业技能竞赛 裁判
* **[2020/09]** 担任 研究生会-学术部长，协作部门成员组织校内活动，同时与其他学生组织、学校部门保持紧密合作
* **[2018/03]** 参加 联合国教科文组织-多语言挑战赛
* **[2016/07]** 担任 全国高校计算机教育大会 志愿者  
  
------

## 内容分享
我会在知乎专栏和 [CSDN](https://blog.csdn.net/qq_41339564) 社区分享一些技术博客，在 [GitHub](https://github.com/realyao) 开源我的代码工作，同时在 [公众号](https://realyao.gitee.io/gzh) REALY 记录个人学习成长经历。

------

## 博客 (building...)

{% include base_path %}
{% capture written_year %}'None'{% endcapture %}
{% for post in site.posts %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% if year != written_year %}
    {% capture written_year %}{{ year }}{% endcapture %}
  {% endif %}
  {% include archive-single.html %}
{% endfor %}