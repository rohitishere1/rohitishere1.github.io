---
layout: post
title: "Tikona Quick Pay"
description: "Bill pament of Tikona ISP made easy"
category: "general" 
tags: ["Tikona","ISP","Payment"]
---
TIKONA ISP BILL PAY MADE EASY
=====================================

I use Tikona ISP for home use. And somehow, I find its bill payment process clumsy. Ironically they spend a good sum in sending reminders in form of sms and auto-tele-calls.

Lets get to Bill Payment,

1. Go to url http://tikona.in/for-home/customer-support/tikona-care/quick-bill-pay/

2. Enter your User Id, on pressing enter it makes an ajax request to fetch your details and a transaction id(hidden). This transaction id is pretty simple and important part, on the ajax request I assume this transaction id gets logged for payment and thie payment is done against this transaction id, which makes this an inevitable step else we could have just jumped to payment and called it real 'Quick Pay'.

3. Now you can enter the amount and pay, by clicking on secure pay. But here is where I always get stuck as many of times nothing happens, and by little bit of debugging i figured out that payment gateway redirection happens on a get request to an IP.

4. So you can guess this step, u need to just enter a url in your browser window and ta-da the payment window opens and you can clear payment and the bill is paid.

5. url is of format http://113.193.1.92:8080/QuickPay/SendInvoke.do?valetId=onlinepay2&password=prod_7498pay2&serviceId=<User Id>&amt=<Amount>&tdnTranId=<Transaction Id>&vrchName=null
The transaction id above is same from step 2. Just paste the url in browser and press enter.
![Tikona Transaction Id](/img/tikona2.jpg)

6. Post step 5. It opens payment options and you can complete the cycle.

For the want of transaction id from step-2 the process would have been shorter, but the site uses csrf tokens so atleast u need to load the page once.

