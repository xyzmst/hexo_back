title: 奇技淫巧之js强券
date: 2016-04-10 09:41:17
tags:
---

1.setInterval()用法：

var idInt = setInterval(function(){},2000);

2.clearInterval用法：

clearInterval(idInt);

以上两个方法结合使用，可以实现实时执行某些功能，并每次执行间隔时间也可以设置。Demo如：
http://blog.csdn.net/majian_1987/article/details/8561855

http://wqs.jd.com/portal/sq/library/index.shtml?PTAG=17054.1.1&DAP=0%3A1689135651692782071%3A2%3A509794&pps=focus.5O201%3A3O7C7623O1FC424O16465O7AB03%3AFO103ED177103ED0098A1F77O200753001B921B79686&_share=mq&_wv=1

```
$(".mod_coupon_xs.type_dong.changestyle")[3].click()


function rob() {  
  var target_time = new Date("2016/04/11,08:59:57");
  var end_time = new Date("2016/04/11,09:05:00");
  var cur_time = new Date();
  if(cur_time.getTime() >= target_time.getTime() && cur_time.getTime() < end_time.getTime())
    $(".mod_coupon_xs.type_dong.changestyle")[3].click()
  setTimeout("rob();", 500);  
}  


function rob() {  
  var target_time = new Date("2016/04/11,00:02:57");
  var end_time = new Date("2016/04/11,00:04:00");
  var cur_time = new Date();
  var a = cur_time.getTime() >= target_time.getTime();
  var b = a && cur_time.getTime() < end_time.getTime();
  if(b){
    console.log(1);
    $(".mod_coupon_xs.type_dong.changestyle")[3].click();
  }
  setTimeout("rob();", 500);  
}  

function rob(){}

  $(document).ready(function(){
    $("p").click(function(){
    $(this).hide();
    });
  });  
```



setTimeout(function(){$(".coupon.small.couponitem")[2].click()}, 1000);  

function rob() {  
  $(".coupon.small.couponitem")[2].click();
  setTimeout("rob();", 1000);  
}  
