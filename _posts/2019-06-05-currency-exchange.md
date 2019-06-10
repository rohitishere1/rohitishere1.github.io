---
layout: post
title: "significant amounts in currency exchange"
description: "dealing with currency exchange in converting to and from and dealing with split payments"
category: "general"
tags: ["currency exchange", "split payments", "Java"]
---

Currency exchange and dealing with them during split payments
=============================================================

Dealing with currency exchange is always tricky but it can not be avoided in circumstances especially when your product has to be scaled in multiple geographies. Let's discuss on how to deal with one such case.
In this use case, consider we are trying to charge a United States currency-account (USD) in Indian Ruppee (INR).

{% highlight bash %}
Base charge amount = 100 INR
Exchange rate = 69.6537 (USD to INR)
Converted charge amount = (100/69.6537) = 1.43567 USD
Now with ISO standards for currency decimal places and rounding off.
Final charge amount = 1.44 USD
{% endhighlight %}

Looks all good and simple, but it gets bad as soon as you involve more complex processes around it like refund and split payment where you might want to allow to split the payment in two payment methods.

So, lets try winding back to do refund.

{% highlight bash %}
Refund amount = 1.44 USD
Exchange rate = 69.6537 (USD to SKW)
Converted refund amount = (1.44 * 69.6537) = 100.3013 INR
After rounding off
Final refund amount = 100.30 INR
{% endhighlight %}

So, with the forward and back calculations we are having a difference of 0.30 INR and this will grow big with different amounts and number of transactions and if split payments is involved it gets more complex. And with all this, the finance and accounting people responsible to balance the sheets are going to be very unhappy, along with the customer who spots this discrepancies.

To tackle this situation we  will first need to decide on rounding off policy when doing currency conversions initially, either we go with rounding up or rounding down. But to decide on which policy to use we need to have the context of use-case, for example, if I am doing this for setting price of an article, I would chose round up policy or else this will be another cost or loss, but if I am doing this for case like split payment where I wish to exhaust balance of an account, I would use round down or else I might end up with accounts with negative balances. After doing this we use these amounts accordingly for calculations (I call them significant amounts).

Let's visualise how this works,
{% highlight bash %}
Base charge amount = 100 INR
Exchange rate = 69.6537 (USD to INR)
Converted charge amount = (100/69.6537) = 1.43567 USD
After rounding down as discussed, 1.43 USD

Now, our final charge
in USD = 1.43 USD
in INR = (1.43 * 69.6537) = 99.6047 ~ 99.60 INR (we can use use rounding policies like round up or round half up here)
{% endhighlight %}

This means our charge amount has to 99.60 INR and not 100 INR, as it will not convert to a significant amount in USD.

A sample code snippet in Java on how to get those converted values,

{% highlight bash %}
public static ImmutablePair<BigDecimal, BigDecimal> calculateChargesinCurrencies (BigDecimal requestAmount,
        String fromCurrency, String toCurrency, BigDecimal exchangeRate, RoundingMode roundingMode) {
        BigDecimal toAmount =
            requestAmount.divide(exchangeRate, exchangeRate.scale(), RoundingMode.HALF_UP).setScale(Currency.getInstance(toCurrency).getDefaultFractionDigits(),
                roundingMode);
        BigDecimal newFromAmount =
            toAmount.multiply(exchangeRate).setScale(Currency.getInstance(fromCurrency).getDefaultFractionDigits(),
                RoundingMode.HALF_UP);
        return ImmutablePair.of(newFromAmount, toAmount);
    }
{% endhighlight %}

This approach will bring down above discussed discrepancies significantly. However this gets more complex when we consider partial refunds, will discuss those in my next post.
