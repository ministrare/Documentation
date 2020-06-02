# Personal Documentation
## CakePHP / "Bin/Cake"

### Index
- [Create a Info User](#Create-a-info-user)
- [Create a Migration](#Create-a-migration)
- [Create a Migration Mark](#Create-a-migration-mark)
- [Clear Cache](#Clear-cache)
- [Fix permission denied on test](#fix-permission-denied-on-test)
- [Run a Migration](#Run-a-migration)
- [Run only Plugin Migrations](#Run-only-migration-plugin)
- [Run a Shell Program](#Run-a-shell-program)
- [Be able to Load images inside the Editor](#Be-able-to-Load-images-inside-the-Editor)
- [Translating PDF](#Translating-PDF)

---

### Create a info user
```
bin/cake migrations seed -p BackCore
```

### Create a migration
```
bin/cake migrations create AlterPostsTable
```
### Create a migration mark
```
bin/cake migrations mark_migrated -p BackStock --target=20190225143409
```
Next:
```
bin/cake core.migrations install
```

### Clear Cache
Copy and paste the next part behind the URL
```
/clear-cache?token=18757c32027188bf372400629967423e55679aec
/clear-core-cache?token=18757c32027188bf372400629967423e55679aec
```
Within the terminal:
```
bin/cake orm_cache clear
bin/cake orm_cache build --connection default
```

### Fix permission denied on test 
```
chmod 0777 bin/cake
```

### Run a migration
```
bin/cake migrations migrate
```
With old programs -> no initial migrations -> error with migrations
```
bin/cake Migrations.migrations migrate
```

### Run only migration plugin
``` 
bin/cake migrations migrate -p BackStock
```

### Run a shell program
```
bin/cake Planning (function name)
```

### Run Seeders
```
bin/cake migrations seed -p BackCore
```

### Be able to Load images inside the Editor
```
bin/cake plugin assets symlink
bin/cake FormManager.Editor CreateFileManagerConfig
```

### Translating PDF
```
bin/cake i18n extract
```
Enter the path, \ Example: ``C:\Source\shipit\back\src\Template\Pdf`` \
Continue pressing enter
