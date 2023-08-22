# postgrel_triger
insert update delete concept
============================================================================
this for update happend in dparent table
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
3) create function for update FUNCTION
CREATE or replace FUNCTION update_child() RETURNS trigger AS
  $BODY$
BEGIN
  UPDATE nir.users_table_temp
  set 
  first_name =new.first_name,
	last_name =new.last_name,
	"password" =new."password",
	username =new.username
  WHERE id = NEW.id;
  RETURN NEW;
END
$BODY$
LANGUAGE plpgsql;

======================================================================
4) create triger for update
CREATE TRIGGER update_child_after_update
AFTER UPDATE 
ON users_table
FOR EACH ROW
EXECUTE PROCEDURE update_child();
