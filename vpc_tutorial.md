## Creating a VPC

1. Select VPC Service by searing it in the searchbar
2. Click `Create VPC`


![create vpc](https://user-images.githubusercontent.com/129324316/234327027-d6346597-7788-43a4-a18b-83a00e00604d.png)


3. We then select `VPC Only`
4. Provide an appropriate name tag such as `fatima-tech221-vpc`
5. Select `IPv4 Manual Input` and enter `10.0.0.0/16`
6. Finally click `Create VPC`


## Creating an Internet Gateway

1. Click on `Internet Gateways`



![crerate internet gateway](https://user-images.githubusercontent.com/129324316/234327130-2ce341a9-4110-4bb7-abf7-71b4ffc9dc6b.png)



2. Provide an appropriate name tag such as `fatima-tech221-ig`
3. Click `Create Internet Gateway`

![gatway](https://user-images.githubusercontent.com/129324316/234327226-75a6835c-cbb3-4a54-b23d-f142c13401ff.png)

## Attaching the Internet Gateway to the VPC
 - There are two ways of doing this: 

1. It will shown an option to connect it to a VPC once you create the gateway


![attach to vpc](https://user-images.githubusercontent.com/129324316/234327323-fb25fe45-a837-4f4f-81d4-23a34c2fb8ce.png)


2. Or, you can click on the `Actions` tab and connect it by clicking `Attach to VPC`


![actions attach](https://user-images.githubusercontent.com/129324316/234327446-965864e5-64e0-421a-ad1c-adc11bb83ec5.png)



## Creating a Subnet


Public Subnet:

1. Click on `Subnets` on the left pane.



![crerate subnet](https://user-images.githubusercontent.com/129324316/234327529-be93b773-b712-4d65-8778-c19826f0e2c0.png)


3. Click `Create Subnet`
4. Select our VPC we made
5. Give the subnet a name such as `fatima-tech221-public-sn`
6. You may set the availability to EU-Ireland or leave it as `no preferance`
7. In the IPv4 CIDR block choose an IP that is compatible with our VPC such as `10.0.4.0/24`

Private Subnet:

1. Copy the same steps as above, except change the name to `fatima-tech221-private-sn`
2. In the IPv4 CIDR Block choose an IP that is compatible with our VPC and is **different** to the public one such as `10.0.0.0/24`




![subnet](https://user-images.githubusercontent.com/129324316/234327659-3e0ed28d-9cb7-478d-b349-5ca788b6e59a.png)




## Creating a Route Table


1. In the left pane you will see `Route Tables`, click on it.


![route table](https://user-images.githubusercontent.com/129324316/234327787-b8e47a0b-3ab7-447c-abd9-6070fd3eb607.png)



2. Give it a name such as `fatima-tech221-private-rt` and `fatima-tech221-public-rt`
7. Select the VPC we created earlier
8. Click `reate Route table`

## Editing the Routing Rules



![create route tabel](https://user-images.githubusercontent.com/129324316/234328012-deb1e062-9a6a-4031-bfe2-ab1a4e705f42.png)



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

![edit subnet](https://user-images.githubusercontent.com/129324316/234327964-47d02319-5252-4806-8499-dcd01ea61fa6.png)




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





![app](https://github.com/fsh-nur/vpc_architecture/assets/129324316/21d281ab-70bc-4c28-9442-5eb27df15fd8)





![fib](https://github.com/fsh-nur/vpc_architecture/assets/129324316/61914081-2863-4ea4-ab38-0a2eb52a861a)





