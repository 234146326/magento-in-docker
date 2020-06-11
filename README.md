# magento-in-docker
magento-in-docker apply magento 2.3.5.p1

## #Prerequisites 

- ### docker
- ###[magento doc](https://devdocs.magento.com/guides/v2.3/install-gde/prereq/nginx.html) 

## #Step1 Clone project

> #### 1, git clone 

- Git 

```$xslt

git clone https://github.com/234146326/magento-in-docker.git && cd magento-in-docker && docker-compose up -d && docker ps 


``` 

- composer (https://packagist.org/packages/jerryxu/magento-in-docker)

```$xslt

composer create-project jerryxu/magento-in-docker && cd magento-in-docker && docker-compose up -d && docker ps

```


> #### 2, goto container 

```$xslt

docker ps && docker exec -it <php-fpm CONTAINER ID> sh

```



## #Step2 Download Project

> ####1, create-project magento []Magento Open Source]


````$xslt

composer create-project --repository=https://repo.magento.com/ magento/project-community-edition public


````

> ***If your network is not good, or you can follow this url document to solve network problems in China.
https://www.cnblogs.com/q1104460935/p/13047522.html

> ####2, goto pubilc dir

##### a), VIM ./phpdocker/nginx/nginx.conf


delete "#" for "# include /application/public/nginx.conf.sample;" ：( Not in php-fpm CONTAINER ID )

```$xslt

include /application/public/nginx.conf.sample;

```

##### b), execution

```$xslt

cd public && touch command_install.sh && chmod +x command_install.sh 

```
> ####3, Find Command_install.sh in public dir.


Write this code：( Not in php-fpm CONTAINER ID )

```$xslt

bin/magento setup:install \
--base-url='http://127.0.0.1:8060' \
--db-host='magento-mysql' \
--db-name='root' \
--db-user='root' \
--db-password='root' \
--backend-frontname='admin' \
--admin-firstname='admin' \
--admin-lastname='admin' \
--admin-email='admin@admin.com' \
--admin-user='admin' \
--admin-password='admin123' \
--language='en_US' \
--currency='USD' \
--timezone='America/Chicago' \
--use-rewrites=1

```

## #Step3   Install magento (in php-fpm CONTAINER ID)

```$xslt

./command_install.sh && php bin/magento setup:upgrade && php bin/magento setup:di:compile && php bin/magento setup:static-content:deploy -f && php bin/magento indexer:reindex && php bin/magento cache:clean && php bin/magento cache:flush && php bin/magento deploy:mode:set developer

```

## # Release production

> Must guarantee you set port is already open in security group and SELinux and iptables has been set up correctly.


### | **File permissions

#### - *When the server fails to run successfully, execute the command to set the necessary permissions for files and folders.


```sybase

find app/code lib pub/static app/etc generated/code generated/metadata var/view_preprocessed \( -type d -or -type f \) -exec chmod g-w {} + && chmod o-rwx app/etc/env.php && find app/code lib var generated vendor pub/static pub/media app/etc \( -type d -or -type f \) -exec chmod g+w {} + && chmod o+rwx app/etc/env.php && chmod 777 -R var && chmod 777 -R generated && chmod 777 -R app/etc && rm -rf var/cache/* var/page_cache/* var/generation/*

```

#### - More  
 
 ( In the case of docker, there is no need to execute this command. )

```sybase

chown -R <U:G> public/*

```

## #More about

Thanks.

- QQ: 1104460935
- Skype: jerryxuying@hotmail,com

