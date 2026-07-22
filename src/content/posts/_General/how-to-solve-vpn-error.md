---
title: THM, HTB VPN Error ဖြေရှင်းနည်း
author: Leo Lwin
pubDatetime: 2025-10-24T17:19:56.768Z
modDatetime: 2025-11-13T13:33:23.236Z
slug: how-to-solve-vpn-error
featured: true
draft: false
tags:
  - general
  - tech
  - VPN
  - Burmese
description: ပြည်တွင်းမှာ လက်ရှိအခြေအနေကြောင့် THM, HTB တို့ရဲ့ OpenVPN ချိတ်မရတာကို ဖြေရှင်းကြပါမယ်။
---

## Table of Contents

## THM, HTB OpenVPN error
  
ကိုယ့် Machine ကိုယ်သုံးရတာ နှစ်သက်တဲ့သူတွေ အတွက်တော့ VPN ချိတ်မရတာက ပြဿနာပါ။  
  
ကိုယ်ပိုင် attackbox တွေ သုံးလို့ရပေမယ့် Try Hack Me မှာဆိုရင် လိုင်းကျသွားတာမျိူးတွေ ရှိတာကြောင့် သုံးရတာနှေးပါတယ်။  
  
Hack The Box ရဲ့ Pwnbox ဆိုရင်လည်း (၂)နာရီအထိပဲ အခမဲ့သုံးလို့ရတယ်။  
  
ဒီတော့ OpenVPN ကို သုံးဖို့ လိုအပ်လာတယ်။  
  
အခုလောလောဆယ် သူ့ချည်းပဲသုံးလို့တော့ မရဘူး။ ပုံကိုကြည့်ပါ။
  
![TLS failed](@/assets/images/VPN_error_tls.png)
  
TLS Handshake fail သွားပါတယ်။  
  
ဒီတော့ ကျနော်တို့အခု...
  
## Bypass ဖို့ပြင်ဆင်ကြမယ်
  
VPN တော်တော်များကို Ban ထားတာကတော့ သိပြီးဖြစ်မှာပါ။ ဒါပေမယ့် ကျနော်တို့ကို ကူညီပေးမယ့် VPN တစ်ခုတော့ ရှိတယ်။  

သူ့ကို download ဆွဲရအောင်။  
  
ထုံးစံအတိုင်း update၊ upgrade လုပ်ပါမယ်။  
  
```
sudo apt update && upgrade -y
```
  
ပြီးရင် VPN ကို download ချမယ်။
  
```
sudo apt install riseup-vpn
```
  
အားလုံးပြီးရင် သူ့ကိုဖွင့်လိုက်ပါ။  
  
Unsecured Connection ဆိုတာကို မြင်ရပါမယ်။ ကျနော်တို့ ဒီအတိုင်းသုံးလို့မရသေးဘူး။  
  
ဘေးက setting ထဲကို အရင်ဝင်ပါ။  
  
![Setting in VPN](@/assets/images/VPN_error_ru_setting.png)
  
Censorship circumvention ဆိုတာကို တွေ့ရပါမယ်။ ကွက်တိပါပဲ။  
  
![Obfs4 before](@/assets/images/VPN_error_obfs4_before.png)
  
ပေးထားတဲ့ options တွေထဲမှာ အပေါ်ဆုံးက Use obfs4 bridges ဆိုတာကိုသုံးမှာပါ။  
  
ဒါပေမယ့် မသုံးခင် ကျနော်တို့ machine ထဲမှာ Tor နဲ့ obfs4proxy တို့လိုတယ်။ 
  
ဒါကြောင့် ကျနော်တို့ ပထမဆုံး...
  
## Tor ကို download ဆွဲမယ်
  
```
sudo apt install tor -y
```
  
Download ချပြီးရင် Tor service ကို start ပါ။
  
```
sudo systemctl start tor
```
  
ပြီးရင် system မှာ run နေသလား သေချာအောင်...
  
```
sudo systemctl status tor
```
  
အခုလိုမြင်ရရင်တော့ အိုကေပါတယ်။
  
![Tor Active](@/assets/images/VPN_error_tor.png)

System boot ပြန်လုပ်ပြီးတိုင်း tor ကို active ဖြစ်နေစေချင်ရင်တော့ enable လုပ်ထားပါ။ မလုပ်လည်း ရပါတယ်။  
  
```
sudo systemctl enable tor
```
  
Tor ကိစ္စပြီးသွားရင်တော့...

## Obfs4proxy ကို download ချမယ်
  
သူကတော့ ရှင်းပါတယ်။
  
```
sudo apt install obfs4proxy -y
```
  
Tor ရော၊ Obfs4proxy ပါ ကိုယ့် machine ထဲရှိနေပြီဆိုရင် ကျနော်တို့...

## VPN စချိတ်ပါမယ်
  
RiseUp VPN ကိုဖွင့်ပါ။  
  
ဘယ်ဘက်က setting ထဲကိုဝင်ပါ။ Use obfs4 bridges ကို ရွေးပါ။  
  
![Obfs4 after](@/assets/images/VPN_error_obfs4_after.png)
  
ပြီးရင် setting ထဲကထွက်ပြီး connect စလုပ်လိုက်ပါ။  
  
တအောင့်လောက်ကြာရင်တော့ Secured Connection ဆိုတာမြင်ရပါမယ်။  
  
ဒါဆိုရင် VPN ချိတ်သွားပြီဖြစ်ပါတယ်။  
  
![VPN connected](@/assets/images/VPN_error_connected.png)
  
ဒါပေမယ့်...

## တကယ်ရော အလုပ်ဖြစ်ရဲ့လား?
  
Google ထဲဝင်ပြီး "where am I" လို့ရိုက်ရှာလိုက်တော့ Paris ဆိုပြီးတွေ့ရတယ်။ ဒါဆို ရသွားပါပြီ။  
  
![Where am I](@/assets/images/VPN_error_where_am_i.png)
  
တစ်ခါတစ်လေ browser ရဲ့ cache တွေကြောင့် location က လက်ရှိနေရာကိုပဲ ပြနေတတ်တယ်။ အဲဒီလိုမျိုးဖြစ်ရင် cache ရှင်းတာမျိုး၊ private or incognito browsing သုံးတာမျိုး လုပ်ကြည့်ပါ။
  
အခု Try Hack Me ရဲ့ OpenVPN ကိုစ,စမ်းပါမယ်။  
  
![THM VPN test](@/assets/images/VPN_error_thm_test.png)
  
အိုကေသွားပါပြီ။ Hack The Box ကိုလည်း ထိုနည်းလည်းကောင်း စမ်းကြည့်ပါ။ အဆင်ပြေပါတယ်။  
  
အခုကျနော်တို့ ပိုသေချာသွားအောင်...

## Try Hack Me ရဲ့ room တစ်ခုကို စမ်းကြည့်ကြမယ်။  
  
RiseUp VPN ကိုဖွင့်၊ Use obfs4 bridges ကိုရွေး၊ connect လုပ်လိုက်ပါ။  
  
ပြီးရင် Try Hack Me ရဲ့ OpenVPN ကို ချိတ်ပါ။  
  
ဒီတစ်ခါတော့ ကျနော်တို့ Try Hack Me ကို တကယ်သုံးမှာ ဖြစ်တဲ့အတွက် OpenVPN ချိတ်ပြီးသွားရင် မူလက RiseUp VPN ကို ပြန်ပြီး Disconnect လုပ်လိုက်ပါ။
  
ဒါဆိုအခု ကျနော်တို့မှာ VPN connection ဆိုလို့ THM OpenVPN တစ်ခုပဲ ရှိတော့မယ်။
  
ပြီးရင် Try Hack Me ထဲက room တစ်ခုကို join ပြီး Start Machine နှိပ်လိုက်ပါမယ်။  
  
![THM room test](@/assets/images/VPN_error_thm_room_test.png)
  
IP ကိုမှတ်ပြီး ping ကြည့်ပါမယ်။ ရရဲ့လား?
  
![IP ping](@/assets/images/VPN_error_ip_ping.png)
  
ping လို့ရတယ်ဆိုရင်တော့ room တွေကို အရင်အတိုင်း ကိုယ့် machine ကိုယ်သုံးပြီး ဖြေလို့ရပြီ ဖြစ်ပါတယ်။
  
ဒါပေမယ့် VPNက တစ်ခါတစ်လေ ချိတ်ဖို့ကြာနေတာ၊ ပြန်ပြုတ်သွားတာမျိုးတွေ ဖြစ်တတ်တယ်။ အဲဒီလို...
  
## Error တွေဖြစ်နေခဲ့ရင် (Updated)
  
- VPN ပြန်ဖြုတ်ပြီး ပြန်ချိတ်ပါ။ ပြီးရင်ခဏစောင့်ပါ။ တချို့စက်တွေမှာကျ Disconnected ဆိုပေမယ့် တကယ်တမ်းကျ Connecting ဖြစ်နေတာပါ။ စက်တစ်ခုနဲ့တစ်ခုတော့ ကွဲနိုင်ပါတယ်။
- RiseUp VPN ထဲကထွက်ပြီး ပြန်ဝင်ပါ။
- စက်ကို reset ပြန်ချကြည့်ပါ။
- စက်ကို update တင်ပါ။ (sudo apt update && upgrade -y)

## နောက်ဆုံးအနေနဲ့ကတော့

RiseUp VPN နဲ့အတူ obfs4 ကြောင့် VPN blocking နဲ့ DPI (Deep Packet Inspection) တွေကို bypass လုပ်လို့ရခဲ့ပါတယ်။  
  
တကယ်က ရှင်းရှင်းလေးပါ။  
  
obfs4 ကြောင့် ကျနော်တို့ရဲ့ VPN traffic တွေက random, unstructured data တွေဖြစ်ကုန်တယ်။  
  
DPI ကလည်း detect လုပ်လို့မရပဲ လွှတ်ပေးလိုက်တဲ့အတွက် network path တစ်ခုဖြစ်လာတယ်။  
  
နောက်တစ်ခါ ထပ်ချိတ်တဲ့ OpenVPN က အဲဒီ network path ကတစ်ဆင့်သွားတဲ့အတွက် ရှောရှောရှူရှူ ချိတ်နိုင်ခဲ့ပါတယ်။  
  
ပိုပြီးလွယ်အောင်ပြောရရင် စာအိတ်တစ်ခုထဲကို နောက်စာအိတ်တစ်ခု ထည့်လိုက်တဲ့သဘောပါ။  
  
ဒီနေရာမှာ ပြောချင်တာက ကျနော်မသိသေးပေမယ့် တခြားအလုပ်ဖြစ်တဲ့ VPN နဲ့ နောက်ထပ်နည်းလမ်းတွေ ရှိကောင်းရှိနေနိုင်ပါတယ်။ အဲဒီအခါကျရင်လည်း ပြန်လည်မျှဝေပေးဖို့ တောင်းဆိုပါတယ်။  
  
အကယ်၍ ဒီ post မှာ အမှားတစ်စုံတစ်ရာ ပါဝင်ခဲ့ရင် ကျနော့်အမှားသာ ဖြစ်ပါတယ်။  
  
အဆုံးထိဖတ်ရှုပေးတဲ့အတွက် ကျေးဇူးပါ။
  
(အခုနည်းလမ်းကို ပြောပြပေးခဲ့တဲ့ brother ကိုလည်း ကျေးဇူးအများကြီးတင်ပါတယ်။)  




