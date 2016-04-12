(暫時紀錄一下，還有些python-mysql的情形也可以分享)
# 緣由

我們都知道MySQL的index很重要，會影響query的速度，設定index的時候，通常直覺會直接對columns(field)直接下index，這樣的做法在一般情形下當然是ok，速度也都還可以接受，但是當資料成長到千萬筆以上的時候，就開始出現問題了，MySQL在index的選擇並不會很聰明地將index合併在一起舉個例子
我有username 和 timestamp 的index，當sql語法下where的時候，MySQL會根據你的資料量來選擇你適合的index
假設以username撈出來會有200萬筆，timestamp撈出來會有500萬筆，這時候MySQL會選擇使用username來做index的key
問題來了，假設符合username以及timestamp的資料可能才3萬筆，每次都要先撈200萬筆以後再去篩選嗎？這樣很慢吧！

所以MySQL又有一種indexing的方法是可以合併多個column當key的，可以把username以及timestamp當做key，這樣MySQL排序篩選的時候，會加快許多，每個framework的設定方法也不一樣，這裡用Django當作例子

[link](https://docs.djangoproject.com/en/1.9/ref/models/options/#index-together)
```
class Meta:
  index_together = [
      ["username", "timestamp"],
  ]
```
這樣即可設定完成

# Reference
indexing 的方法有兩種，也有些差異，可以參考
[這裡](https://www.percona.com/blog/2014/01/03/multiple-column-index-vs-multiple-indexes-with-mysql-56/)
