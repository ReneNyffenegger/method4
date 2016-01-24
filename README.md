`METHOD4`
============

Method4 is a PL/SQL application to run dynamic SQL in SQL.

## Example

The simplest way to call Method4 is to pass in a simple literal query to evaluate:

        select * from table(method4.run(q'[select * from dual]'));
        
        D
        -
        X

At first that query seems pointless - why not just directly query `select * from dual`?  Method4 allows custom code to control how the code is run and what is returned.

As a simple and useful example, Method4 includes a mode that runs queries generated by queries.  This can solve challenging problems such as "count the rows for every table".

To enable this re-evaluation mode, set the parameter `p_re_eval` to YES.  In this mode, Method4 runs a query that returns a query string.  Those queries are concatenated with UNION ALL and then run.

        select * from table(method4.run(
        	p_stmt =>
        	q'[
        		select 'select '''||table_name||''' table_name, count(*) a from '||table_name sql
        		from user_tables
        		where table_name like 'TEST%'
        	]',
        	p_re_eval => 'YES'
        ));
        
        TABLE_NAME                         A
        ------------------------- ----------
        TEST                           19765
        TEST1                              1
        TEST2                              1
        TEST3                              1
        ...

## Notes

Method4 is based on the Dictionary Long Application, (c) Adrian Billington www.oracle-developer.net.  Much of this code contains advanced methods thoroughly discussed on his website, http://www.oracle-developer.net/display.php?id=422

Method4 is a simpler, more generic version of that application.  It can be useful for adhoc queries in highly dynamic environments.  For example, an application where the schemas, tables, and columns are table-driven and only known at run time.

Use this program with caution.  Few programs need to be this dynamic.  This package will be slower and buggier than regular SQL.

## Installation

Click the "Download ZIP" button, extract the files, CD to the directory with those files, connect to SQL*Plus, and run these commands:

1. Install Method4:

        @install

2. Unintall Method4:

        @uninstall

3. Install unit tests (optional, only useful for development):

        @tests/install_unit_tests

4. Uninstall unit tests (optional, only useful for development):

        @tests/uninstall_unit_tests
