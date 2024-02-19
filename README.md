## polars_partitions

**GitHub:** [polars_partitions](https://github.com/dwenlvov/polars_partitions)  
**Version**: 0.1.1

### Python
```
pip install polars_partitions
```

## Description
This library is not a replacement for [Polars](https://pola.rs/).
The main goal is to improve the work (write/read/filter) with partitions by creating a Table Of Contents file (hereinafter referred to as "TOC").

### Write Partition
### polars_parquet.wr_partition() 
**polars_parquet.wr_partition(**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _df_: DataFrame,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _columns_: array | string,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _output_path_: str  
**)**

#### Parameters
**df**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Polars DataFrame  
**columns**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Array of columns on which to create partitions  
**output_path**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Path to save to  

### TOC record
### polars_parquet.wr_toc() 
**polars_parquet.wr_toc(**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _df_: DataFrame on which the partitions are based,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _columns_: array | string,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _output_path_: str  
**)**

#### Parameters
**df**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Dictionary, where the key is the column and the array is the values  
**columns**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Array of columns to create partitions for  
**output_path**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Path to save to  

### Reading TOC
### polars_parquet.rd_toc() 
**polars_parquet.rd_toc(**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _output_path_: DataFrame,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _filters_: dict = None,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _btwn_: str = None  
**)**

#### Parameters
**output_path**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Path where to save.  
**filters**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Dictionary, where the key is the column and the array is the values  
**btwn**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Works in conjunction with **filters**. It takes as input the **column name** on which to apply the **between** filter. It takes the first two values from the filters(array).  

### Read Partition
### polars_parquet.rd_partition() 
**polars_parquet.rd_partition(**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _output_path_: str,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _columns_: array | string = "*",  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _filters_: dict = None,  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; _btwn_: str = None  
**)** → LazyFrame  

#### Parameters
**output_path**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Path to the parquet file or to the partitions folder  
**columns**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Array of columns to return  
**filters**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Dictionary where the key is the column and the array is the values  
**btwn**  
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Works in conjunction with **filters**. It takes as input the **column name** on which to apply the **between** filter. It takes the first two values from the filters(array).  
## How to use (example)
``` python
import polars_partitions as plp
from datetime import date
import polars as pl

# Create a test dataset
df = pl.DataFrame({'col1':[date(2024,1,1),date(2024,1,1),date(2024,1,2),date(2024,1,2),date(2024,1,2),date(2024,1,3),date(2024,1,3),date(2024,1,3)],
              'col2':['A2','A2','A2','A2','A2','A2','B2','B2','B2','B2'],
              'col3':[1,2,3,4,5,6,7,8]
              })

output_path = 'your_path/folder_name_where_to_save'.
# Which columns are partitioned by
columns = ['col1', 'col2'] 

pp = plp.polars_partitions()

# Write the partitions
pp.wr_partition(df, columns, output_path)

# Read TOC
# print(pp.rd_toc(output_path))

# Read partitions and apply filters
# filters = {'col1':[date(2024,1,1),date(2024,1,3)]}
# df = pp.rd_partition(output_path, filters=filters, btwn='col1', columns=['col1', 'col3']) 
# print(df.collect())
```