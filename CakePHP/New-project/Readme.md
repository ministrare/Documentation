# Personal Documentation
## CakePHP / "New Project"

### Index
- [Back Skeleton](#Back-skeleton)
- [Creating a new project](#Creating-a-new-project)
- [Create a info user](#create-a-info-user)
- [](./)

### Back skeleton
```
composer create-project digitalcompany/back-skeleton back --repository-url=https://packages.digitaltalents.be --stability=dev
```

### Creating a new project
```git
git clone --depth=1 https://gitlab.com/digitaltalents/projectnaam.git
```

### Create a info user
```
bin/cake migrations seed -p BackCore
```