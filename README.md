# CS-GY 9163 Extra Credit

## Problem Statement

> For this EC: find a recent security problem and try to scrutinize  it for accuracy. Is it scaremongering to make more sales? Is it due to a lack of understanding of the tech? Maybe it's accurate?  

## Security Problem

Many news outlets reported on a data breach at Capital One yesterday (2019/07/30) with reports such as [this one](https://www.cnn.com/2019/07/30/tech/data-breach-capital-one-hack/index.html)

> Millions of Capital One customers have been affected by a data breach that the bank says happened in March when a software engineer allegedly exploited a vulnerability to access its systems.

There is no doubt that a large amount of PII (Personally Identifiable Information) was leaked, so while the titles of many of these articles trend click-baity, there is certainly some truth behind it. What isn't made clear in most headlines is what data was specifically exposed. According to a [Capital One statement](https://www.capitalone.com/facts2019/), log-in credentials and credit card numbers were not captured, and less than 1% of SSNs were leaked. Still, over 140,000 US SSNs were leaked as part of information obtained through credit applications over a 14 year period from 2005 to 2019, including bank account numbers, and Canadian SINs. In order to scrutinize for accuracy I'd like to take a look at exactly what the *hack* was that *exploited a vulnerability*.

Krebs on Security also has a great article on this that [goes a little deeper](https://krebsonsecurity.com/2019/07/capital-one-data-theft-impacts-106m-people/) where we learn about the hackers previous employment at Amazon (Specifically in the S3 area, which was the destination of the attack) and about the hackers activity and incriminating messages left in other open spaces (Twitter, Meetup, Slack). This article points out a file, `ISRM-WAF-ROLE.tar.xz`, and names it as the one that belonged to Capital One but leaves it at that.

Further investigation to this attack will lead to the [court filing](https://www.dropbox.com/s/z7u5rxcdajuvw6t/19718675504.pdf?dl=0) for the case. In this we learn that...

> Capital One examined (a) GitHub file, ... determined that the April 21 File contained the IP address for a specific server. A firewall misconfiguration permitted commands to reach and be executed by that server, which enabled access to folders or buckets of data...

Based on the archive name (ISRM-WAF-ROLE) the best I can assume is that this improperly secured server had credential access for the aforementioned role (likely by some way of caching or potentially by way of AWS Instance Profiles) and this role was used to sync (exfil) the contents of the PII-containing-buckets.

## Conclusion

I don't think any of the news hype was scaremongering, there was definitely a large leak of PII and I don't believe this benefits anyone (maybe perimeter security as a service companies?). I think the only bits missing are the more technical pieces that contributed to the leak: 

* Why such broad access to this role? 
* Why such a powerful role? (Named for AWS WAF presumably but has access to all these S3 buckets?) 
* How can we audit to prevent misconfiguration?
* How can we utilize an attacker's arrogance for earlier detection?  The (NLP scanning of tweets? Image processing of screenshots?)

It's a good lesson in IAM/ISRM management, it had accurate reporting around it, and it's good consumers are aware so they can take steps to secure themselves. Finally, a [hilarious tweet](https://twitter.com/cwarzel/status/1156178208003252225) commenting on Capital One's PR:

![Le Tweet](https://pbs.twimg.com/media/EAuR80sUwAIEEzF.jpg)
