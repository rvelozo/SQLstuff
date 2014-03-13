
```
select
	u.UserID,
	isnull(ui.InheritedUserID ,0) as InheritedUserID
from
	tblUser u
left outer join
	tblUserInheritance  ui on
	u.UserID = ui.UserID
;
with tblChain
as(
	select
		ui.UserID,
		ui.UserID,
		ui.InheritedUserID,
		cast((ui.UserID) as nvarchar (100)) as "Inheritance Chain"
	from @tblInheritance  ui
	where
		ui.UserID=@UserIDInheriting
	union all
	
	select
		chain.UserID,
		uiloop.userid,
		uiloop.inheriteduserid,
		CAST((chain.[Inheritance Chain]  + ',' + cast((uiloop.UserID) as nvarchar(100))) as nvarchar (100)) as "Inheritance Chain"
	from 
		@tblInheritance as uiloop
	join
		tblChain chain on
		chain.InheritedUserID = uiloop.UserID 
)

SELECT @InheritanceChain = COALESCE(@InheritanceChain + ',', '') + tc.[Inheritance Chain] 
FROM tblChain tc
```
