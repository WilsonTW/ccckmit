#字串的誤用 — (String Misuse)

無法決定長度的字串，請不要用標準函式庫，要改用動態字串。

在 Java 這樣的語言當中，字串的長度是可以改變的，您可能會使用下列程式，很自由的讓字串大小增大，Java 會自動的幫您分配字串所需要的記憶空間，而不會產生太大的問題。

###範例一、字串連接的 Java 程式

```java
String s = "";
for (i=0; i<100; i++)
  s = s + token[i];
```

但是在 C 語言當中，您就會遇到一個困擾，假如我要撰寫一個類似的程式，那麼字串 s 的長度應該要設定為多長呢？請看下列範例。

###範例二、字串連接的 C 程式

```c
char s[1000];
for (i=0; i<100; i++)
  strcat(s, token[i]);
```
您可能會懷疑，長度 1000 到底夠不夠呢？假如 token 陣列中的字串長度平均超過 10 個字，那麼 s 的長度 1000 就會不夠了。這樣看來，Java 的字串函式庫設計似乎比 C 語言要好得多了，不是嗎？

如果您不夠理解 C 語言，就很可能會寫出像範例二這樣的程式，但是這對 C 語言來說其實是一種誤用，誤用的原因是：「想要用靜態的字串處理動態的問題」。

假如您真的必需撰寫像範例二這樣的程式，那麼應該自行設計一個動態的字串函式庫，或者採用某個現成的動態字串函式庫，而不是直接用 C 語言內建的標準函數。但是 C 語言的初學者往往沒有這樣一個函式庫，因而寫出像範例二這樣的程式。

程度稍好的資訊系學生，可能理解到這個問題不能採用靜態的解決方式，因此使用 malloc() 函數進行記憶體分配，以解決這個令人困擾的問題，於是就寫出了下列的程式碼。

###範例三、字串連接的 C 程式 (malloc 版)

```c
char *s = malloc(1);
s[0] = '\0';
for (i=0; i<100; i++) {
  char *t = malloc(strlen(s)+strlen(token[i])+1);
  sprintf(t, "%s%s", s, token[i]);
  free(s);
  s = t;
}
```

但是這樣撰寫 C 語言，仍然是初學者會犯的錯誤之ㄧ，這種用法完全誤用了 C 語言的能力，造成了很多次的 malloc() 分配，卻又很沒效率的處理字串長度的成長問題。

###正確的用法

這種情況應該改用動態字串，請進一步下列文章

* 動態字串物件 -- (Dynamic String) 可以動態增長的字串物件，讓您不用再為字串長度傷腦筋。

