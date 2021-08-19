# SQLSTR

主要用于从SQL中解析出所有表名、视图名(结果去重去空)  

## Examples

### Clean Query from double white space, comment, etc.

```go
cleaned := sqlstr.Clean(`
  SELECT *
  FROM table -- some table comment 
  WHERE column1 = 'meh' /* request from tyrion*/`)

fmt.Println(cleaned) 

// Output: 
// SELECT * FROM table WHERE column1 = 'meh'
```
### Obscure value 

```go
obsecured := sqlstr.Obscure(`SELECT * FROM table WHERE column1 = 'text' AND column2 = 1234 AND column3 = TRUE and column4 = 3.14`)

fmt.Println(obsecured)

// Output: 
// SELECT * FROM table WHERE column1 = ? AND column2 = ? AND column3 = ? and column4 = ?
```

### Get Table Names

```go
queryString := sqlstr.NewQueryString(`SELECT a.mc, a.hm
FROM (
	SELECT n.dj, coalesce(n.shx, n.sr) AS hx, h.HYMC
	FROM DJ.V_MC n, HX_QG.V_DM_GY_HY h
	WHERE n.KQCBZ = 'N'
		AND n.yx = 'Y'
		AND n.KZ_DM < '1140'
		AND n.DM = h.DM
) a	JOIN (
		SELECT c.dj, SUM(CASE WHEN c.nd = '2021' THEN c.jsyj ELSE 0	END) AS xs21
		FROM (
			SELECT s.dj, s.jsyj, extract(year FROM s.SKSSQQ) AS nd
			FROM HX_SB.v_SB s
			WHERE S.SKSQ >= DATE '2016-01-01'
				AND S.SKSQ <= DATE '2021-06-30'
		) c
		GROUP BY c.dj
	) b ON a.dj = b.dj;`)

tableNames := queryString.TableNames()

fmt.Println(tableNames)

// Output:
// [DJ.V_MC  HX_QG.V_DM_GY_HY  HX_SB.v_SB]
```
