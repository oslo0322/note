(暫時紀錄一下，還有些python-mysql的情形也可以分享)
# 緣由

大家都知道MySQL的index很重要，會影響query的速度
可能大家在設定index的時候，就直接對columns(field)直接下index，比較快也比較直覺。

這樣的做法在一般情形下當然是ok，速度也都還可以接受
但是當資料成長到千萬筆以上的時候，就開始出現問題了

indexing 的方法有兩種，也有些差異，可以參考
[這裡](https://www.percona.com/blog/2014/01/03/multiple-column-index-vs-multiple-indexes-with-mysql-56/)
