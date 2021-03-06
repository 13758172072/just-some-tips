#RegExp学习总结

##字符集合（Character Sets）
* [xyz] 字符组|字符集合， 匹配集合中的任意一个字符， 可以使用连字符'-'来指定一个范围。 如```[abcd]```可以表示为```[a-d]```。
* [^xyz] 反义字符组， 匹配中括号中不存在的内容
##字符边界 (Boundaries)
* ^  匹配字符串开头
* $  匹配字符串结尾
* \b 匹配一个零宽单词边界 如一个字母和一个空格之间， 一个字符串的开头和结尾
    
  ```js
  'et'.match(/\bet/); // ["et"]
  'testfsad'.match(/\bet/) // null
  ```
* \B 匹配一个零宽非单词边界， 如两个字母之间或两个空格之间
  
  ```js
  '   '.match(/\B/); // four white-spaces
  // ["", "", "", ""];

  ```
##分组(Grouping) 和 反向引用(back references)
* ```(x)```  

  匹配x并且捕获匹配项。 捕获组(Capturing groups)有性能惩罚， 再次访问被捕获的子字符串最好使用非捕获符号
   
   ```js
   var str = 'Yes, I wanna earn more money';
   /(\S)*\s/.test(str); 
   RegExp.$1 // ","
   
   /(\S*)\s/.test(str);
   RegExp.$1 // '"Yes,"
   ```
* ```\n```  
  n是一个正整数。 指向正则表达式中第n个括号（从左边开始数）中匹配的字符串 **这里有点问题**
* ```(?:x)```  
  匹配x不会捕获匹配项， 非匹配项不能通过RegExp的$1-$9属性访问到。 
##数量词
* ```x*```   
  匹配前面的模式x 0次或多次
* ```x+```  
  匹配前面的模式x 1次或多次， 等价于{1,}
* ```x*?```, ```x+?```  
  匹配前面的模式， 但是是执行最小可能匹配
   
  ```js
  var str = "Yes, I wanna earn more money";
  str.match(/\w*?/); // [""]    // 最小匹配模式， 可匹配0次
  str.match(/\w*/);  // ["Yes"]  

  str.match(/\w+/);  // ["Yes"]
  str.match(/\w+?/); // ["Y"]   // 最小匹配模式， 只匹配1次
  ```
* ```x?```  
  匹配前面的模式x 0次或1次 。 如果在数量词```*```, ```+```, ```?```或者```{}```任意一个后面添加该符号（?）, 将会使匹配模式变为非贪婪模式(non-greedy)， 即匹配次数最小化， 默认匹配模式是贪婪模式， 即匹配次数最大化。 

* ```x(?=y)```
  只有当x后面紧跟着y的时候才会进行匹配， 但是y不会成为匹配内容的一部分  . 注意要加括号
  ```js
  var str = 'hello world, I feel so good today';
  str.match(/feel(?= so)/); // ["feel"]
  

  ``` 

* ```x(?!y)```
  只有当x后面不是紧跟着y的时候才会进行匹配
  
  ```js
  str.match(/feel(?!so)/); // ["feel"] // 注意： 中间没有空格
  ```

* ```x|y```
  匹配x或者y  
  ```js
  str.match(/hello|world/);         // ["hello"]

  // 此时匹配的是[]内的每一个可能值， 只匹配1次的话就匹配到了'h'， 而此时'|'也只被当成一个普通的字符
  str.match(/[hello|world]/);       // ["h"] 
  'jfsdalk|sajd'.match(/[fa|fj]/g); // ["j", "f", "a", "|", "a", "j"]
  ```
* ```x{n}```  
  n是一个正整数， 前面的模式连续出现n次时匹配

* ```x{n,}```
* ```x{n,y}```
   **Note:** 这里n和y之间不能有空格  

  ```js
  // 从url中提取子域名
  var url = "http://xxx.domain.com";
  url.replace(/https?:\/\/([^.]*)\..*/, '$1'); // "xxx"
  ```
