# DDL Postgres Macros

In postgres you may find yourself repeating common operations on all your tables. For example adding an audit log ,created times, modified user etc.

This can be tedious to add everywhere. Instead, you can create a postgres "macro" to update your DDL for you


# Example
## Created
```postgresql
CREATE OR REPLACE FUNCTION public.add_created_columns(  
    table_type regclass  
) RETURNS void  
    LANGUAGE plpgsql  
    SET search_path TO ''  
AS $_$  
declare  
    statement text = format($$  
        alter table %1$s 
            add column created_user int references public."user"(id) not null,
            add column created_at   timestamptz not null default current_timestamp;
        $$,  
        $1);
begin  
    execute statement;  
end;  
$_$;
```

### Usage

To use the `add_created_columns` macro, you simply call it by passing the table name as a parameter. Here's an example
of how to use it:

```postgresql
SELECT public.add_created_columns('public.orders'),
       public.add_created_columns('public.products');
```

This will modify the specified table by adding the `created_user` and `created_at` columns as defined in the macro.

## Modified


To add similar columns for tracking modifications, you can create a new macro called `add_modified_columns`. Here's an
example:

```postgresql
   create or replace function update_modified_at()
    returns trigger as $$
begin
    new.modified_at = current_timestamp;
    return new;
end;
$$ language plpgsql;


CREATE OR REPLACE FUNCTION public.add_modified_columns(  
table_type regclass  
) RETURNS void  
LANGUAGE plpgsql  
SET search_path TO ''  
AS $_$  
declare  
statement text = format($$  
        alter table %1$s
            add column modified_user int references public."user"(id) not null,
            add column modified_at timestamptz not null default current_timestamp;
    
        create trigger update_modified_at_trigger
            before update on %1$s
            for each row
            execute function update_modified_at();
        $$,  
        $1);

begin  
execute statement;  
end;  
$_$;
```
### Usage

To use the `add_modified_columns` macro, you can call it similarly as follows:

```postgresql
SELECT public.add_modified_columns('public.orders'),
       public.add_modified_columns('public.products');
```

This will modify the specified table by adding the `modified_user` and `modified_at` columns.
