# SqlMap


## 目标
> 使用 SqlMap 主要目的是使我们的sql语句可运维，运维部门可以直接查看我们的 SqlMap 了解我们业务中使用的sql语句，对sql语句进行优化，索引优化，还可以对慢查询进行快速定位。当业务发布新的 SqlMap 可以通过运维预审核，避免出现线上慢查询。


## 要求
> 所有sql语句都要写在 SqlMap 里，非特殊情况，不允许使用#WHERE 标签。必须要明确的sql语句。
> 例子：
``` php
   SELECT * FROM market_category WHERE market_id=#{market_id}  and parent_id= #{parent_id}  AND category_name= #{category_name}
```
禁止使用：
``` php
   SELECT * FROM market_category WHERE #WHERE
```

## 用法

### insert

### batch
``` php
      INSERT INTO market_goods #INSERTS#;
```      
### update

### delete

### select








