[[defaults-main]]  
[[handlers-main]]  
[[meta-main]] 
[[tasks-main]]
[[templates-test.py.j2]]  
[[tests-inventory]]  
[[tests-test]] 
[[vars-main]]  
[[vars-requirements]]
[[vars.travis]] 

```
#!/usr/bin/env python

import cx_Oracle
import config as cfg

sql = """Enter multi-value sql here"""

    try:
        # establish a new connection
        with cx_Oracle.connect(cfg.username,
                            cfg.password,
                            cfg.dsn,
                            encoding=cfg.encoding) as connection:
            # create a cursor
            with connection.cursor() as cursor:
                # execute the insert statement
                cursor.execute(sql)
                # commit work
                connection.commit()

    except cx_Oracle.Error as error:
        print('Error occurred:')
        print(error)
        connection.rollback()
        connection.close()

    finally:
    # release the connection
    if connection:
        connection.close()
```


## Change md extension to just py.j2 to make it a runnable python script