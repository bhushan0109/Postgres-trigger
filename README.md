# Postgres trigger
Postgres insert or update trigger WHEN condition 
===================================================================================================================
IF YOU WANT TO audit ON TABLE OR MANUPULATION ON TABLE THEN I HAVE CREATED TRIGERS FOR UPDATE OR INSERT happend on parent table
==================================================================================================================
PART 1--> 
this for UPDAET OR INSERT happend in parent table
==========================================================================
1) create parent table
CREATE TABLE nir.users_table (
	id serial4 NOT NULL,
	first_name varchar(255) NULL,
	last_name varchar(255) NULL,
	"password" varchar(255) NULL,
	username varchar(255) NULL,
	CONSTRAINT users_table_pkey PRIMARY KEY (id)
);
====================================================================

2)create temp table
CREATE TABLE nir.users_table_temp (
	id serial4 NOT NULL,
	first_name varchar(255) NULL,
	last_name varchar(255) NULL,
	"password" varchar(255) NULL,
	username varchar(255) NULL,
	CONSTRAINT users_table_temp_pkey PRIMARY KEY (id)
);
=========================================================================
3) FUCTION IS CREATED

CREATE OR REPLACE FUNCTION mytrigger()
  RETURNS trigger AS
$BODY$
begin
     --code for Insert
     if  (TG_OP = 'INSERT') then
          INSERT INTO nir.users_table_temp SELECT (NEW).*;  
 		  RETURN NEW;
     end if;

     --code for update
     if  (TG_OP = 'UPDATE') then
         UPDATE nir.users_table_temp
  		 set 
  			first_name =new.first_name,
			last_name =new.last_name,
			"password" =new."password",
			username =new.username
 		 WHERE id = NEW.id;
 		 RETURN NEW;
      end if;
return new;
end;
$BODY$
  LANGUAGE plpgsql VOLATILE

  ==============================================================================================================================
4) TRIGER IS CREATED
  CREATE TRIGGER mytrigger
    BEFORE INSERT OR UPDATE ON "users_table"
    FOR EACH ROW 
    EXECUTE PROCEDURE mytrigger();

================================================================================================================================
					Thanks for looking out
-==============================================================================================================================











