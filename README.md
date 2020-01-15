## TT-CONNECTOR
![](https://hackerx.org/wp-content/uploads/job-manager-uploads/company_logo/2017/11/TrackTik-Logo-800x200.png)
#### Prerequisite
- Docker
- Vendor Client Secret Key
- Vendor Client ID
- Vendor API Hosts
- Vendor API Authentication Grant Type
- Vendor Access Key File;
- Vendor Access Certificates

#### Deployment

- Clone the source using https://github.com/TrackTik/tt-connector.git
- Build Docker Containers

```javascript
cd <project_directory>
docker-compose build
```

- Now start the containers


```javascript
docker-compose up -d
```
- You can also go through the logs while building containers if required.

```javascript
docker-compose logs -f
```
- Now you need to execute migrations.
-- Log in to the php container. You can get container names from the following command
```javascript
docker container ls
```

```javascript
docker-compose exec -it <php_container_name> sh
php bin/console doctrine:migrations:migrate
```
Now you will be able to see tables in mysql database.

- Apply Vendor Certificates and Keys
-- You need to paste **certificate.pem** and** privatekey.key** file in **certificates** directory in project root.  You can obtain these files from the development team .

- Apply Vendor Client Credentials
 -- You need to add Vendor client credentials and certificate path in database fixture .
```php
# src/DataFixtures/VendorFixtures.php
		...........
		.............
           $credentials->setClientSecret($type.'_CLIENT_SECRET_XXXX');
            $credentials->setClientID($type.'_CLIENT_ID_XXXX');
            $credentials->setApiHost($type.'_API_HOST_XXXX');
            $credentials->setGrantType($type.'_GRANT_TYPE_XXXX');
            $credentials->setType($type);
            $credentials->setKeyPath('KEY_PATH/XXXX/XXXXX.key');
            $credentials->setCertificatePath('CERT/XXXX/XXXX.pem');
		...........
		.............
```
- Now you need to execute Fixture to fill credentials in database. Execute the following command in php container

```javascript
php bin/console doctrine:fixtures:load
```
Now you run the connector application by going to
https://localhost/{{vendor}}/connect

