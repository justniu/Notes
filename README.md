### 2020-10-27 高级数据库技术
```
DECLARE  
emp employee %rowtype; //一条元组作为对象?  
BEGIN  
EXCEPTION  
END;  
阻抗失配问题，定义游标；类似缓存，存储多条满足条件的元组。  

Declare  
  theld char(3);  
  CURSOR c IS; //?    
BEGIN  
  OPEN c;  
    ......  
    CLOSE c;  
END;  
```