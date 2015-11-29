# 當AWS ELB 碰到 Nginx

首先問題是這樣的，Nginx 當你使用pass_proxy的時候，如果你後面直接使用ELB給你的DNS Name(不管你後面有沒有把DNS name做改寫)，過了數小時之後，你會發現很多request會開始轉發失敗而導致502

## ELB
原因是這樣的，我們先來談談ELB的IP機制
ELB 給你DNS Name的原因是因為，他每過數個小時之後會更換IP，而且一個DNS Name，至少會有兩個以上的IP
這種機制不管是在 internal或者public ELB上都是一樣的

## Nginx
Nginx在使用 pass_proxy，如果後面直接給予一個DNS name的話，他會先去Query IP，然後把它儲存起來
而儲存的期限如同...鑽石恆久遠，IP永流傳，不會自動地更新

---
綜合以上兩者機制，所以產生了這篇文章

解法是將轉向的網址設成變數，這樣每次他的Cache只會儲存5分鐘(Stack Overflow 上看到的說法)
```
resolver 10.0.0.0+2/16 # 可以參考AWS DNS (註1);
set $upstream_endpoint http://xxxx.elb.amazonaws.com;
location / {
    proxy_pass $upstream_endpoint;
}
```
Nginx pass_roxy 裡面有提到

```
A server name, its port and the passed URI can also be specified using variables:

proxy_pass http://$host$uri;
or even like this:

proxy_pass $request;
In this case, the server name is searched among the described server groups, and, if not found, is determined using a resolver.
```

註1.http://docs.aws.amazon.com/zh_cn/AmazonVPC/latest/UserGuide/VPC_DHCP_Options.html#AmazonDNS

參考 http://tenzer.dk/nginx-with-dynamic-upstreams/
