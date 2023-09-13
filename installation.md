## Obtain public and private keys from the Adobe account

> [https://commercedeveloper.adobe.com/account/keys](https://commercedeveloper.adobe.com/account/keys ':target=_blank')


## Get the Magento meta-package by the composer

```
 composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.4.6 m246
```

## Install the sample data (if required)

```
 bin/magento sampledata:deploy
```

## Install Magento by command

```
bin/magento setup:install --backend-frontname=backend --base-url=http://baseurl/ \
--db-host=localhost --db-name=db_name --db-user=db_user --db-password=db_password \
--admin-firstname=Magento --admin-lastname=User --admin-email=email@gmail.com \
--admin-user=user_name --admin-password=password --language=en_US \
--currency=USD --timezone=America/Chicago --use-rewrites=1 \
--search-engine=elasticsearch7 --elasticsearch-host=localhost \
--elasticsearch-port=9200

```

## Disable TwoFactorAuth module 

```
bin/magento module:disable Magento_AdminAdobeImsTwoFactorAuth Magento_TwoFactorAuth

```