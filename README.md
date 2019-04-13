# SQL---Trigger-SPROC-Example
SQL - Trigger / SPROC Example




/* I don't know what column you want to put this in.  It wasn't clear so I created one.*/
alter table Warfare_Forces
  add max_unit_position int(3) default 0 not null;
-- ----------------------------------------
/* Create a proc that takes force id as a variable and updates the max unit position*/

CREATE PROCEDURE proc_warfare_force_set_max_unit_position(vForceID int)
BEGIN
  update Warfare_Forces set Warfare_Forces.max_unit_position = ifnull(
    select max(Position) from Warfare_Units where ForceID = vForceID
    ,0);
END;

-- -----------------------------------------
/* Set triggers to fire anytime a unit's position gets updated */

CREATE TRIGGER trig_warfare_unit_insert
AFTER INSERT
   ON Warfare_Units FOR EACH ROW
BEGIN
   -- variable declarations
  declare vForceID int(3);
   -- trigger code
  call proc_warfare_force_set_max_unit_position(vForceID);
END;

CREATE TRIGGER trig_warfare_unit_update
AFTER UPDATE
   ON Warfare_Units FOR EACH ROW
BEGIN
   -- variable declarations
  declare vForceID int(3);
   -- trigger code
  call proc_warfare_force_set_max_unit_position(vForceID);
END;

CREATE TRIGGER trig_warfare_unit_delete
AFTER DELETE
   ON Warfare_Units FOR EACH ROW
BEGIN
   -- variable declarations
  declare vForceID int(3);
   -- trigger code
  call proc_warfare_force_set_max_unit_position(vForceID);
END;
