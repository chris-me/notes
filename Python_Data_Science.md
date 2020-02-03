## general

### importing

Import some file ('dburl.py') from one folder upwards, then one folder deep.

```python
import sys 
sys.path.append('../foo')
import dburl
url = dburl.get_db_url()
```

## pandas

### change display options

```python
pd.set_option('display.max_rows', 100)
pd.set_option('display.max_columns', 20)
pd.set_option('display.max_colwidth', 100)
pd.set_option('display.width', 240)
```
