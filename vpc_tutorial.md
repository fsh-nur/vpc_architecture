## Creating a VPC

1. Select VPC Service by searing it in the searchbar
2. Click `Create VPC`
3. We then select `VPC Only`
4. Provide an appropriate name tag such as `fatima-tech221-vpc`
5. Select `IPv4 Manual Input` and enter `10.0.0.0/16`
6. Finally click `Create VPC`


## Creating an Internet Gateway

1. Click on `Internet Gateways`
2. Provide an appropriate name tag such as `fatima-tech221-ig`
3. Click `Create Internet Gateway`


## Attaching the Internet Gateway to the VPC
 - There are two ways of doing this: 

1. It will shown an option to connect it to a VPC once you create the gateway

2. Or, you can click on the `Actions` tab and connect it by clicking `Attach to VPC`

## Creating a Subnet 

Public Subnet:

1. Click on `Subnets` on the left pane.
2. Click `Create Subnet`
3. Select our VPC we made
4. Give the subnet a name such as `fatima-tech221-public-sn`
5. You may set the availability to EU-Ireland or leave it as `no preferance`
6. In the IPv4 CIDR block choose an IP that is compatible with our VPC such as `10.0.4.0/24`

Private Subnet:

1. Copy the same steps as above, except change the name to `fatima-tech221-private-sn`
2. In the IPv4 CIDR Block choose an IP that is compatible with our VPC and is **different** to the public one such as `10.0.0.0/24`


## Creating a Route Table

1. In the left pane you will see `Route Tables`, click on it.
2. Give it a name such as `fatima-tech221-private-rt` and `fatima-tech221-public-rt`
3. Select the VPC we created earlier
4. Click `reate Route table`

## Editing the Routing Rules

1. Click on `Edit Routes` in the bottom corner
2. There will already be a route present to our VPC
3. Now we need to allow internet access for the **public** subnet and we add:

```
Destination; 0.0.0.0/0
Target: Internet Gateway
```
4. For the **private** subnet we add:

```
Destination: Public Subnet IP
Target: Instance (Our App)
```
5. Click on `subnet association`
6. Associate the respective subnet with the respective route.
7. Click `Save assosciaton`

## Create an EC2 Instance in our VPC

1. Launch the instance from the AMI we created earlier for the db instance and the app instance
2. Edit Network settings and select our VPC which we made
3. Select the respective subnets
4. Create a new security group for each instance
5. For the **public-app** instance:
*The `auto-assign public IP` should be enabled*

```
SSH for PORT 22 from MY IP
HTTP access for PORT 80 from 0.0.0.0/0
Custom TCP for PORT 3000 from 0.0.0.0/0
```
6. For the **private-db** instance:
*The `auto-assign public IP` should be **disabled***

```
Custom TCP for PORT 27017 from PUBLIC SUBNET IP
SSH for PORT 22 from MY IP
```
## Connecting the APP to DB in terminal

1. SSH into the app instance
2. Edit .bashrc
```
sudo nano .bashrc
```
3. Change the IP in DB_HOST environmental variable:
```
 export DB_HOST=mongodb://<DB INSTANCE PRIVATE IP>:27017/posts
```
4. Navigate to the app and launch it
```
cd app
node app.js
```
5. You should see the posts page available too















