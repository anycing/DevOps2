# select

#### select\_demo1.sh

```sh
#!/bin/bash

select language in c python shell
do
  echo $language
done
```

![](<../../.gitbook/assets/image (10).png>)



#### select\_demo2.sh

```sh
#!/bin/bash

PS3="What is your preffered scripting language? "

select language in bash perl python ruby quit
do
  case $language in 
    bash | perl | python | ruby ) echo "You selected $language" ;;
    quit ) exit ;;
    * ) echo "You selected error, retry ..." ;;
  esac
done
```

![](<../../.gitbook/assets/image (67).png>)



#### select\_demo3.sh

```sh
#!/bin/bash

PS3="What is your favorite OS? "
IFS="|"

os="Linux|Windows|Mac OS"
select item_os in $os
do
  case $REPLY in
    1 | 2 | 3 ) echo "You selected $item_os" ;;
    * ) exit ;;
  esac
done
```

![](<../../.gitbook/assets/image (20).png>)



#### select\_demo4.sh

```sh
#!/bin/bash

PS3="Select a program you want to execute: "
p_list="nginx mysql php"

select program in $p_list quit
do
  [[ $program == quit ]] && exit
  rpm -q $program > /dev/null && echo $program || echo "$program is not installed"
done
```

![](<../../.gitbook/assets/image (77).png>)
