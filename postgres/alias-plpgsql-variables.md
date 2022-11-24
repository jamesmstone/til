# Alias plpgsql variables

Inside plpgsql function you can often get naming conflicts between input parameters and column /  table names

My previous modus operandi was to qualify the input param using the function name. For example:

## Schema
```plpgsql
create table data.user (
  id serial primary key;
  name text not null;
  birth_date date not null;
);
```
## Options
### Qualify using function name
```plpgsql
create function age( "user" int ) returns int as 
$$
  declare
    user_id alias for "user";
    --  user_id alias for $1; -- alternative
  begin
    select age(current_date, birth_date) from data.user where id = age.user ;
  end;
$$ language plpgsql;

```
### ALIAS

But you can also use `ALIAS FOR`:

```plpgsql
create function age( "user" int ) returns int as 
$$
  declare
    user_id alias for "user";
    --  user_id alias for $1; -- alternative
  begin
    select age(current_date, birth_date) from data.user where id = user_id ;
  end;
$$ language plpgsql;
```

**Aside:** For a function that only does a single `select` a `SQL` language function is more appropriate.  
